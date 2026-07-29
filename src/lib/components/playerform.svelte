<script>
    let playername = $state("");
    let playerrating = $state("");

    let { onadd, editply, onupdate } = $props();

    $effect(() => {
        if (editply) {
            playername = editply.name;
            playerrating = editply.rating;
        }
    });

    function submit() {
        if (editply) {
            onupdate(editply.id, playername, playerrating);
        } else {
            let id = crypto.randomUUID();
            onadd(id, playername, playerrating);
        }

        playername = "";
        playerrating = "";
    }
</script>

<div>
    NAME:
    <input
        type="text"
        placeholder="enter your name"
        bind:value={playername}
    >
    rating:
    <input
        type="number"
        placeholder="enter your rating"
        bind:value={playerrating}
    >

    <button onclick={submit}>
        {editply ? "UPDATE" : "ADD"}
    </button>
</div>
