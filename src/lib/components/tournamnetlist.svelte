<script>
let {tournaments,ondelete,onedit}=$props();
function dltt(id){
    ondelete(id);
}
function edtt(id,name){
    onedit(id,name);
}
</script>

<h1>no of tournaments={tournaments.length}</h1>
{#each tournaments as tour}
    <div>
    <p>{tour.name}</p> -- <button onclick={()=>{edtt(tour.id,tour.name)}}>EDIT</button> <button onclick={()=>{dltt(tour.id)}}>DELETE</button>
    </div>
{/each}
