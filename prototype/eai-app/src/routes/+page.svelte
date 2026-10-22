<script>
  import "bulma/css/bulma.min.css";
  import "@fortawesome/fontawesome-free/css/all.css";
	import Commands from '../components/Commands.svelte';
</script>

<section class="hero is-fullheight ether">
  <div class="block">
    <div class="columns is-centered is-mobile">
      <div class="column custom-sophia-bg parent-container">
        <img class="idle" src="/images/EAI-Logo.png" width="120px" height="120px" />
        <Commands/>
      </div>
    </div>
  </div>
</section>

<style>
  .parent-container img {
    position: absolute;
    top: 71%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 120px;
    height: 120px;
  }

  .custom-sophia-bg {
    background-image: url("/images/sophia-full.png");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: top center;
    border: 2px solid #527083; /* Example border style */
    border-radius: 10px; /* Example border radius */
    max-width: 500px;
    min-height: 360px;
    margin-top: 30px;
    height: 90vh;
    /* Additional styles can be applied here */
  }

  .parent-container {
    position: relative; /* Ensure the parent container is relatively positioned */
  }

  .ether {
    background-color: #162643;
    background-size: 40px 40px;
    background-image: linear-gradient(to right, #527083 1px, transparent 1px),
      linear-gradient(to bottom, #527083 1px, transparent 1px);
  }

	/* Animation for the pulsing effect */
	@keyframes pulse {
		0% {
			opacity: 0.2;
		}
		50% {
			opacity: 1;
		}
		100% {
			opacity: 0.2;
		}
	}

	/* Applying the pulsing effect */
	.interacting {
		animation: pulse 0.5s infinite;
	} 

  .idle {
		animation: pulse 1.5s infinite;
	} 

</style>
