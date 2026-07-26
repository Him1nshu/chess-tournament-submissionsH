<script>
let {tournaments,players,ondelete,onedit,onaddplayer,onremoveplayer}=$props();
let selected=$state({});

function dltt(id){
    ondelete(id);
}
function edtt(id,name){
    onedit(id,name);
}
function addp(tourId){
    let playerId=selected[tourId];
    if(playerId){
        onaddplayer(tourId,playerId);
    }
}
function playername(id){
    let player=players.find((p) => p.id === id);
    return player ? player.name : "unknown";
}
</script>

<h1>no of tournaments={tournaments.length}</h1>
{#each tournaments as tour}
    <div>
        <p>{tour.name}</p> -- <button onclick={()=>{edtt(tour.id,tour.name)}}>EDIT</button> <button onclick={()=>{dltt(tour.id)}}>DELETE</button>

        <select bind:value={selected[tour.id]}>
            <option value={undefined} disabled selected>select player</option>
            {#each players as player}
                <option value={player.id}>{player.name}</option>
            {/each}
        </select>
        <button onclick={()=>{addp(tour.id)}}>add player to tournament</button>

        <ul>
            {#each tour.playerIds as playerId}
                <li>{playername(playerId)} <button onclick={()=>{onremoveplayer(tour.id,playerId)}}>remove</button></li>
            {/each}
        </ul>
    </div>
{/each}
