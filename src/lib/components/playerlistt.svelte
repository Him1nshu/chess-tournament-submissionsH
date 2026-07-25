<script> 
let {players,ondelete,onedit}=$props();
function dltp(id){
    ondelete(id);
}
function edtp(id,name,rating){
    onedit(id,name,rating);
}
</script>

<h1>no of players={players.length}</h1>
{#each players as player}
    <div>
    <p>{player.name} - {player.rating}</p> -- <button onclick={()=>{edtp(player.id,player.name,player.rating)}}>EDIT</button> <button onclick={()=>{dltp(player.id)}}>DELETE</button>
    </div>
{/each}