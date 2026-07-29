<script>
let {matches,tournaments}=$props();

function tourname(id){
    let tour=tournaments.find((t) => t.id === id);
    return tour ? tour.name : "unknown";
}
function maxround(tourId){
    let rounds=matches.filter((m) => m.tourId === tourId).map((m) => m.round);
    return Math.max(...rounds);
}
function winnername(match){
    return match.p2 ? (match.winnerId === match.p1.id ? match.p1.name : match.p2.name) : match.p1.name;
}
</script>

<h1>no of matches={matches.length}</h1>
{#each matches as match}
    <div>
        <p>{tourname(match.tourId)} - {match.round === maxround(match.tourId) ? "FINAL" : `round ${match.round}`}: {match.p1.name} vs {match.p2 ? match.p2.name : "BYE"}</p>
        <p>winner: {winnername(match)}</p>
    </div>
{/each}
