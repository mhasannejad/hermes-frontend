<script>
    import axios from "axios";
    import {baseProfsUrl, baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {createEventDispatcher, onMount} from "svelte";
    import {closeModal} from "svelte-modals";

    export let isOpen
    export let professor_id

    let contactTypeList = []
    onMount(() => {
        axios({
            url: `${baseProfsUrl}contacttypes/all/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            contactTypeList = r.data
            console.log(r)
        })
    })

    let contact_obj = {
        name: '',
        value: '',
        type_id: NaN
    }

    let dispatcher = createEventDispatcher()
    const sendContactObj = () => {
        axios({
            url: `${baseProfsUrl}contacts/add/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(contact_obj)
        }).then(r => {
            if (r.status === 200) {
                closeModal()
                dispatcher('added')
            }
            console.log(r.data)
        })
    }

</script>
{#if isOpen}
    <div class="modal">
        <div class="contents">

            <h4 class="text-dark">Add New Contact</h4>
            <form on:submit|preventDefault={sendContactObj}>
                <select bind:value={contact_obj.type_id} class="form-select my-3" aria-label="Default select example ">
                    {#each contactTypeList as contact,index}
                        <option value={contact.id} class="my-2">
                            {contact.name}
                        </option>
                    {/each}

                </select>
                <div class="mb-3">
                    <label class="form-label">Name</label>
                    <input bind:value={contact_obj.name} type="text" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label">Value</label>
                    <input bind:value={contact_obj.value} type="text" class="form-control">
                </div>
                <button type="submit" class="btn btn-primary w-100">send</button>
            </form>
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
        background: white;
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
