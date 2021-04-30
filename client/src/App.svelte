<script>
import Router from "svelte-spa-router";
import Homepage from "./routes/Homepage.svelte";
import Userpanel from "./routes/Userpanel.svelte";
import StartSession from "./routes/StartSession.svelte";
import ResultsSession from "./routes/ResultsSession.svelte";
import RegisterPage from "./routes/RegisterPage.svelte";
import ActiveSession from "./routes/ActiveSession.svelte";
import QuestionPage from "./routes/QuestionPage.svelte";

const routes = {'/': Homepage,
'/userpanel': Userpanel, 
'/startsession': StartSession, 
'/resultssession': ResultsSession,
'/registerpage': RegisterPage,
'/activesession': ActiveSession,
'/questionpage' : QuestionPage };
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