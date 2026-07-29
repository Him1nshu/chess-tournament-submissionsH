<script>
let tname=$state("")
    let { onadd, edittour, onupdate }=$props();
    $effect(() => {
        if (edittour) {
            tname = edittour.name;
        }
    });

    function addtn(){
        if(edittour){
            onupdate(edittour.id, tname);
        } else {
            let id = crypto.randomUUID();
            onadd(id, tname);
        }
        tname="";
    }
</script>


TOURNAMENT NAME:
<input type="text" placeholder="enter toune name" bind:value={tname}>
<br>
<button onclick={addtn}>{edittour ? "UPDATE" : "create"}</button>
