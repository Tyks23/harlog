<script>
import Router from "svelte-spa-router";
import Homepage from "./routes/Homepage.svelte";
import Userpanel from "./routes/Userpanel.svelte";
import StartSession from "./routes/StartSession.svelte";
import ResultsSession from "./routes/ResultsSession.svelte";

const routes = {'/': Homepage,
'/userpanel': Userpanel, 
'/startsession': StartSession, 
'/resultSsession': ResultsSession };
</script>
<div class="router-div">
<Router {routes}/>
</div>

<style>
.router-div {
	margin: 70px 170px;
	display: flex;
	align-items: center;
	justify-content: center;
}

</style>