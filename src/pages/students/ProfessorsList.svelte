<script lang="ts">
    import Chip, {Set, Text} from '@smui/chips';
    import {onMount} from "svelte";
    import axios from "axios";
    import {userD} from '../../utils/auth.js'
    import {baseStudentsUrl} from '../../utils/consts.js'
    import ProffessorVertical from "../../components/components/ProffessorVertical.svelte";

    let filters = {
        expertise: [],
        groups: [],
        locations: []
    }
    let filters_selected = {
        expertise: [],
        groups: [],
        locations: []
    }

    let professors = []

    onMount(() => {
        axios({
            url: baseStudentsUrl + 'filter/all/professors/',
            method: "POST",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            console.log(r.data)
            filters = r.data
        })
    })
    $:{

        axios({
            url: `${baseStudentsUrl}professors/filtered/`,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(filters_selected)
        }).then(r => {
            if (r.status === 200) {
                professors = r.data
                console.log(r.data)
            }
        })
    }

</script>





<div class="container">
    <Set chips={filters.expertise} let:chip filter bind:selected={filters_selected.expertise}>
        <Chip {chip} touch>
            <Text>{chip.name}</Text>
        </Chip>
    </Set>
    <Set chips={filters.locations} let:chip filter bind:selected={filters_selected.locations}>
        <Chip {chip} touch>
            <Text>{chip.name}</Text>
        </Chip>
    </Set>
    <Set chips={filters.groups} let:chip filter bind:selected={filters_selected.groups}>
        <Chip {chip} touch>
            <Text>{chip.name}</Text>
        </Chip>
    </Set>
    <h4  class="section-header">
        <i class="bi bi-list text-light"></i> Professors
    </h4>
    <div style="" class="row">
        {#each professors as prof}
            <ProffessorVertical proffesor={prof} />
        {/each}

    </div>
</div>

<style>
    * {
        color: white !important;
    }
</style>
