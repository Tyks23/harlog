<script>
import {push} from "svelte-spa-router";
let roomExists = false;

</script>
<div class="container">
{#if !roomExists}
    <h1>Harlog</h1>
    <div>
        <h2 class="title">Küsitluses osalemine</h2>
        <p>Sisesta ruumivõti:</p>
        <form class="container">
        <input type="text" placeholder="Võti">
        <button on:click|preventDefault="{() =>{roomExists=true}}">SISESTA</button>
        </form>
    </div>
    <div>
        <h2 class="title">Küsitluste haldamine</h2>
        <p>Logi sisse</p>
        <form class="container">
        <input type="text" placeholder="Kasutajatunnus">
        <input type="password" placeholder="Parool">
        <input type="submit" value="SISENE" on:click|preventDefault="{() => {push('/userpanel')}}">     
        </form>
    </div>
    <input type="button" value="Registreeru">
{:else}
<div>
    <h1>Harlog</h1>
    <h1 class="title">Küsitluses osalemine</h1>
    <form class="container">
    <p>Sisesta nimi</p>
    <input type="text" placeholder="Nimi">
    <p>Sisesta email</p>
    <input type="text" placeholder="Email">
    <input type="submit" value="SISENE">     
    </form>
</div>
{/if}
</div>


<style>
.container {  
    display: flex;
    flex-direction: column;
}
	
</style>