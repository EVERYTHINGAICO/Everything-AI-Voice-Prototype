<script>
	import { onMount } from 'svelte';
	import { ChatGroq } from '@langchain/groq';
	import { ChatPromptTemplate } from '@langchain/core/prompts';
	import axios from 'axios';

	let status = 'idle';
	let verbose = false;

	let transcript = '';
	let displayResponse = '';
	let audioUrl = '';
	let audioElement;
	let pastMessages = [
		[
			'system',
			'Sophia is a friendly and efficient voice assistant. It responds in a very brief way, encouraging a conversation style. All responses will be converted into voice, so no emojis nor markup symbols.'
		]
	];

	const model = new ChatGroq({
		apiKey: 'REMOVED_FROM_PUBLIC_ARCHIVE',
		modelName: 'llama2-70b-4096'
	});

	const tester = new ChatGroq({
		apiKey: 'REMOVED_FROM_PUBLIC_ARCHIVE',
		modelName: 'mixtral-8x7b-32768',
		stop: ['STOP']
	});

	async function replayAudio() {
		// Stop any currently playing audio
		if (audioElement && !audioElement.paused) {
			audioElement.pause();
		}

		// Create an audio element and set its source to the received audio URL
		audioElement = new Audio(audioUrl);

		// Play the audio
		audioElement.play();
	}

	// Use the LLM to detect if last message is talking to Sophia

	async function wakeUp(receivedTranscript) {
		const prompt = ChatPromptTemplate.fromMessages([
			[
				'system',
				"Respond with TRUE STOP or FALSE STOP only, a single character. Detect if the user is talking to this assistant. The name of the assistant is Sophia, but it also accepts variants like Sofia, Sophie or 'assistant'. Return 'TRUE STOP' if the sentence contains any type of expression that is attemtping to grab Sophia's attention, examples are 'Sophia', 'Hey, Sophia' or 'Can you help me Sophia?'. Return 'FALSE STOP' if the sentence does not seem to be directed to Sophia."
			],
			[
				'human',
				"You are answering a boolean question about a prompt. The question will either be true/yes/affirmative, or false/no/negative. IMPORTANT: If yes or true or affirmative, return 'TRUE STOP'. If no or false or negative, return 'FALSE STOP'. Detect if the user wants to start a conversation with the assistant. Here is the input: {input}"
			]
		]);
		const chain = prompt.pipe(tester);
		const response = await chain.invoke({
			input: receivedTranscript
		});
		console.log('Wake Up: ', response.content);

		if (response.content.trim() == 'TRUE') {
			status = 'active';
			pastMessages.push(['human', receivedTranscript]);
			responseLLM();
		}
	}

	async function gotoSleep(receivedTranscript) {
		const prompt = ChatPromptTemplate.fromMessages([
			[
				'system',
				"Respond with TRUE STOP or FALSE STOP only, a single character. Detect if the user does not want to talk anymore with this assistant. The name of the assistant is Sophia, but it also accepts variants like Sofia, Sophie or 'assistant'. Return 'TRUE' if the sentence contains any type of expression that means that the conversation is over or should pause, examples are 'Thank you Sophia', 'Hold on', 'Please, stop', or 'That will be all.' Return 'FALSE' if the sentence does not seem to be finishing or pausing the conversation with Sophia."
			],
			[
				'human',
				"You are answering a boolean question about a prompt. The question will either be true/yes/affirmative, or false/no/negative. IMPORTANT: If yes or true or affirmative, return 'TRUE STOP'. If no or false or negative, return 'FALSE STOP'. Detect if the user does not want to talk anymore with the assistant. Here is the input: {input}"
			]
		]);
		const chain = prompt.pipe(tester);
		const response = await chain.invoke({
			input: receivedTranscript
		});
		console.log('Sleep: ', response.content);

		if (response.content.trim() == 'TRUE') {
			status = 'idle';
		}
		pastMessages.push(['human', receivedTranscript + "(Sophia, finish the conversation)"]);
		responseLLM();
	}

	async function responseLLM() {
		console.log(pastMessages);

		const prompt = ChatPromptTemplate.fromMessages([...pastMessages, ['human', '{input}']]);

		const chain = prompt.pipe(model);
		const response = await chain.invoke({
			input: transcript
		});
		console.log('response', response);
		transcript = '';

		// Convert Assistant Response into an Audio and Play it
		let voiceRequestData = {
			model: 'aura-asteria-en',
			input: response.content
		};

		const voiceResponse = await axios.post(
			'https://example.invalid/historical-text-to-speech-endpoint',
			voiceRequestData,
			{
				responseType: 'blob' // Tell axios to expect a binary response
			}
		);

		// Create a URL for the received audio blob
		audioUrl = window.URL.createObjectURL(voiceResponse.data);

		displayResponse = response.content;
		pastMessages.push(['ai', displayResponse]);
		replayAudio();
	}

	onMount(() => {
		// Historical archive: microphone and external calls are disabled.
		const archiveOnly = true;
		if (archiveOnly) return;
		navigator.mediaDevices.getUserMedia({ audio: true }).then((stream) => {
			const mediaRecorder = new MediaRecorder(stream);
			const socket = new WebSocket('wss://api.deepgram.com/v1/listen?punctuate=true', [
				'token',
				'REMOVED_FROM_PUBLIC_ARCHIVE'
			]);

			socket.onopen = () => {
				// console.log({ event: "onopen" });
				mediaRecorder.addEventListener('dataavailable', (event) => {
					if (event.data.size > 0 && socket.readyState == 1) {
						socket.send(event.data);
					}
				});
				mediaRecorder.start(250);
			};

			socket.onmessage = (message) => {
				const received = JSON.parse(message.data);
				const receivedTranscript = received.channel.alternatives[0].transcript;
				if (receivedTranscript && received.speech_final) {
					console.log(receivedTranscript);
					// Stop any currently playing audio
					if (audioElement && !audioElement.paused) {
						audioElement.pause();
					}

					//Testing for waking up or sleep mode
					if (status == 'idle') {
						wakeUp(receivedTranscript);
					} else if (status == 'active') {
						gotoSleep(receivedTranscript);
					}

					// pastMessages.push(["human", receivedTranscript]);
					// responseLLM();
				}
			};

			socket.onclose = () => {
				console.log({ event: 'onclose' });
			};

			socket.onerror = (error) => {
				console.log({ event: 'onerror', error });
			};
		});
	});
</script>

{#if displayResponse}
	<div class="has-text-white columns is-mobile commands">
		<div class="column">
			<h2 class="subtitle is-4 has-text-white has-text-centered">
				{displayResponse}
			</h2>
		</div>
	</div>
{/if}

<style>
	.commands {
		position: absolute; /* Position the commands element absolutely */
		width: 90%;
		bottom: 0; /* Align the bottom edge of the commands element with the bottom of the parent container */
		margin: 20px; /* Optional margin from the bottom of the parent container */
		background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.3)); /* Example gradient */
		border: 4px solid #8a9fab; /* Example border style */
		border-radius: 10px; /* Example border radius */
	}
</style>
