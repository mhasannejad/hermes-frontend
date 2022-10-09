<script>
    import axios from "axios";
    import {baseProfsUrl, baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {TimePicker, TimePickerModal} from "svelte-time-picker";
    import {DateInput} from "date-picker-svelte";
    import {closeModal} from "svelte-modals";
    import {toast} from "@zerodevx/svelte-toast";

    export let isOpen
    export let announcement

    let proposalObj = {

        announce_id: announcement.id,
        description:''
    }
    const setUp = () => {
        axios({
            url: `${baseStudentsUrl}announcerequest/create/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(proposalObj)
        }).then(r => {
            if (r.status === 201) {
                toast.push('proposal sent')
                closeModal()
            }
            console.log(r.data)
        })
    }


</script>
{#if isOpen}
    <div class="modal">
        <div class="contents">

            <div class="mb-3">
                <label  class="form-label"> write a proposal for {announcement.title}</label>
                <textarea class="form-control" rows="3" bind:value={proposalObj.description}></textarea>
            </div>
            <button on:click={setUp} class="appointment-button">
                Send Proposal
            </button>
        </div>
    </div>
{/if}

<style>
    .modal {
        position: fixed;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;

        /* allow click-through to backdrop */
        pointer-events: none;
    }

    .modal > * {
        color: white;
    }

    .contents {
        min-width: 400px;
        border-radius: 6px;
        padding: 16px;
        background: #fff;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        pointer-events: auto;
    }

    h2 {
        text-align: center;
        font-size: 24px;
    }

    p {
        text-align: center;
        margin-top: 16px;
    }

    .actions {
        margin-top: 32px;
        display: flex;
        justify-content: flex-end;
    }
</style>
