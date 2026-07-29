<script>
		import Playerform from "$lib/components/playerform.svelte";
		import Playerlistt from "$lib/components/playerlistt.svelte";
		import Tournamnetform from "$lib/components/tournamnetform.svelte";
		import Tournamnetlist from "$lib/components/tournamnetlist.svelte";
		import Matchlist from "$lib/components/matchlist.svelte";

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
		let matches=$state([]);

		function addtour(id,name){
			let obj={id,name,playerIds:[],status:"pending"};
			tournaments=[...tournaments,obj];
		}
function starttour(id) {
	tournaments = tournaments.map((tour) =>
		tour.id === id ? { ...tour, status: "started" } : tour
	);

	let selectedTournament = tournaments.find((tour) => tour.id === id);
	//console.log(selectedTournament);

	let tournamentplyrs = players.filter((player) =>
		selectedTournament.playerIds.includes(player.id)
	);
	//console.log(tournamentplyrs);

	let round = 1;
	let current = tournamentplyrs;
	let allMatches = [];

	while (current.length > 1) {
		let shuffled = [...current];
		for (let i = shuffled.length - 1; i > 0; i--) {
			let j = Math.floor(Math.random() * (i + 1));
			[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
		}

		let roundMatches = [];
		for (let i = 0; i < shuffled.length; i = i + 2) {
			let match = { p1: shuffled[i], p2: shuffled[i + 1] };
			roundMatches = [...roundMatches, match];
		}

		let winners = [];
		for (let i = 0; i < roundMatches.length; i++) {
			let pp1 = roundMatches[i].p1;
			let pp2 = roundMatches[i].p2;

			roundMatches[i].id = crypto.randomUUID();
			roundMatches[i].tourId = id;
			roundMatches[i].round = round;

			if (!pp2) {
				roundMatches[i].winnerId = pp1.id;
				winners = [...winners, pp1];
				continue;
			}

			let rndval = Math.random();
			let winner = rndval >= 0.5 ? pp1 : pp2;
			roundMatches[i].winnerId = winner.id;
			winners = [...winners, winner];
		}
		//console.log("round", round, roundMatches);

		allMatches = [...allMatches, ...roundMatches];
		current = winners;
		round = round + 1;
	}

	let championId = current.length === 1 ? current[0].id : null;

	let finalRound = round - 2;
	let finalMatch = allMatches.find((m) => m.round === finalRound);
	let runnerUpId = finalMatch
		? finalMatch.winnerId === finalMatch.p1.id ? finalMatch.p2.id : finalMatch.p1.id
		: null;

	let semiMatches = allMatches.filter((m) => m.round === finalRound - 1);
	let thirdPlaceIds = [];
	for (let i = 0; i < semiMatches.length; i++) {
		let m = semiMatches[i];
		if (m.p2) {
			let loserId = m.winnerId === m.p1.id ? m.p2.id : m.p1.id;
			thirdPlaceIds = [...thirdPlaceIds, loserId];
		}
	}

	matches = [...matches, ...allMatches];

	tournaments = tournaments.map((tour) =>
		tour.id === id ? { ...tour, status: "completed", championId, runnerUpId, thirdPlaceIds } : tour
	);
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
		function addplayertotour(tourId,playerId){
			tournaments = tournaments.map((tour) =>
				tour.id === tourId && !tour.playerIds.includes(playerId)
					? { ...tour, playerIds: [...tour.playerIds, playerId] }
					: tour
			);
		}
		function removeplayerfromtour(tourId,playerId){
			tournaments = tournaments.map((tour) =>
				tour.id === tourId
					? { ...tour, playerIds: tour.playerIds.filter((id) => id !== playerId) }
					: tour
			);
		}
</script>
<main>
	<header>
		<h1>Chess Tournament Manager</h1>
		<p>Manage players, tournaments, matches and rankings</p>
	</header>

	<section>
		<h2>Player Management</h2>
		<Playerform onadd={addplayer} editply={editply} onupdate={updateplayer}/>
		<Playerlistt players={players} ondelete={deleteplayer} onedit={editplayer} />
	</section>

	<section>
		<h2>Tournament Management</h2>
		<Tournamnetform onadd={addtour} edittour={edittour} onupdate={updatetour} />
		<Tournamnetlist tournaments={tournaments} players={players} ondelete={deletetour} onedit={edittournament} onaddplayer={addplayertotour} onremoveplayer={removeplayerfromtour} onstart={starttour} />
	</section>

	<section>
		<h2>Match Results</h2>
		<Matchlist matches={matches} tournaments={tournaments} />
	</section>
</main>

<style>
	main {
    max-width: 1100px;
    margin: 0 auto;
    padding: 32px 20px;
}
	section {
    margin: 24px 0;
    padding: 24px;
    border: 1px solid white;
    border-radius: 10px;
}
	main{
    margin: 0;
    font-family: Arial, sans-serif;
}
</style>