<script>
	let winr=$state("");
	let plyrs=$state(["magnus","padmananda","unniksrihsnan"]);
	function addinlist() {
		plyrs=[...plyrs,winr];
		winr="";
	}
	
</script>
<style>
    h1{
        color:red;
    }
	h2{
		color: green;
	}
</style>
<h1>CHESS TOURNAMENT</h1>
<hr>
<h2>add in the new player name</h2>

<input type="text" placeholder="enter short name" bind:value={(winr)}>
<button onclick={addinlist}>add in name</button>
<hr>
<ol>
	
{#each plyrs as p}
	<li><h3>{p}</h3></li>
{/each}
</ol>


