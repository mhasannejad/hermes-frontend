<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import ProffessorVertical from "../../components/components/ProffessorVertical.svelte";

    console.log(baseUrl)
    let homeData = {
        profs: []
    }
    onMount(() => {
        axios({
            url: `${baseStudentsUrl}home/`,
            method: 'GET'
        }).then(r => {
            homeData = r.data
            console.log(r)
        })
    })
</script>

<div class="container">
    <h4  class="section-header">
        <i class="bi bi-list text-light"></i> Professors
    </h4>
    <div style="" class="row">
        {#each homeData.profs as prof}
            <ProffessorVertical proffesor={prof} />
        {/each}

    </div>
</div>

