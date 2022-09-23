<script>
    import axios from "axios";
    import {baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";

    export let isOpen
    export let professor_id
    let appointment_obj = {
        title: '',
        description: '',
        professor_id:professor_id
    }
    const requestAppointment = () => {
        axios({
            url: `${baseStudentsUrl}appointments/create/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(appointment_obj)
        }).then(r => {
            if (r.status === 200) {
                isOpen = false
            }
            console.log(r.data)
        })
    }

</script>
{#if isOpen}
    <div class="modal">
        <div class="contents">

            <h4 class="text-dark">request appointment</h4>
            <form on:submit|preventDefault={requestAppointment}>
                <div class="mb-3">
                    <label class="form-label">Title</label>
                    <input bind:value={appointment_obj.title} type="text" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label">Description</label>
                    <textarea rows="4" bind:value={appointment_obj.description}  class="form-control"></textarea>
                </div>
                <p class="text-dark">appointed time will be sent to you by email</p>
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
