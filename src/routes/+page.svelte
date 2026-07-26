<!-- <script>
	let winr=$state("");
	let plyrs=$state(["magnus","padmananda","unniksrihsnan"]);
	function addinlist() {
        if(winr.trim()!=""){
		plyrs=[...plyrs,winr];
		winr="";
        
	}
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

	-->
<script>
		import Playerform from "$lib/components/playerform.svelte";
		import Playerlistt from "$lib/components/playerlistt.svelte";
		import Tournamnetform from "$lib/components/tournamnetform.svelte";
		import Tournamnetlist from "$lib/components/tournamnetlist.svelte";
		
		let players=$state([]);
		let editply=$state(null);

		function addplayer(id,name,rating){
			let obj={id,name,rating};
			players=[...players,obj];
		}
		function deleteplayer(id){
			players=players.filter((player) => player.id !== id)
		}
		function editplayer(id,name,rating){
			editply={id,name,rating};
		}
		function updateplayer(id, name, rating) {
        players = players.map((player) =>
            player.id === id
                ? { ...player, name, rating }
                : player
        );
        editply = null;
    	}
		
		let tournaments=$state([]);
		let edittour=$state(null);

		function addtour(id,name){
			let obj={id,name};
			tournaments=[...tournaments,obj];
		}
		function deletetour(id){
			tournaments=tournaments.filter((tour) => tour.id !== id)
		}
		function edittournament(id,name){
			edittour={id,name};
		}
		function updatetour(id,name){
			tournaments = tournaments.map((tour) =>
				tour.id === id
					? { ...tour, name }
					: tour
			);

			edittour = null;
		}
</script>
	<Playerform onadd={addplayer} editply={editply} onupdate={updateplayer}/>
	<Playerlistt players={players} ondelete={deleteplayer} onedit={editplayer} />
	<br>
	<Tournamnetform onadd={addtour} edittour={edittour} onupdate={updatetour} />
	<Tournamnetlist tournaments={tournaments} ondelete={deletetour} onedit={edittournament} />