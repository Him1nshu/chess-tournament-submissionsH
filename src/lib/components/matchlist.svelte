<script>
let {matches,tournaments,onsetwinner}=$props();

function setw(matchId,winnerId){
    onsetwinner(matchId,winnerId);
}
function tourname(id){
    let tour=tournaments.find((t) => t.id === id);
    return tour ? tour.name : "unknown";
}
</script>

<h1>no of matches={matches.length}</h1>
{#each matches as match}
    <div>
        <p>{tourname(match.tourId)} - round {match.round}: {match.p1.name} vs {match.p2 ? match.p2.name : "BYE"}</p>
        <p>winner: {match.p2 ? (match.winnerId === match.p1.id ? match.p1.name : match.p2.name) : match.p1.name}</p>
        {#if match.p2}
            <button onclick={()=>{setw(match.id,match.p1.id)}}>set {match.p1.name} as winner</button>
            <button onclick={()=>{setw(match.id,match.p2.id)}}>set {match.p2.name} as winner</button>
        {/if}
    </div>
{/each}
