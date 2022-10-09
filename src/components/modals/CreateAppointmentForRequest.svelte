<script>
    import axios from "axios";
    import {baseProfsUrl, baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {DateInput} from "date-picker-svelte";
    import {toast} from "@zerodevx/svelte-toast";
    import {closeModal} from "svelte-modals";

    export let isOpen
    export let student
    export let announce


    let date_time = new Date()
    let appointment_obj = {
        title: `appointment for ${announce.title}`,
        description: '',
        announce_id: announce.id,
        student_id: student.id,
        time:Math.round(date_time.getTime()/1000),
    }

    const submit = () => {
        axios({
            url: `${baseProfsUrl}appointments/build/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(appointment_obj)
        }).then(r => {
            if (r.status === 200) {
                toast.push('appointment built')
                closeModal()
            }
            console.log(r.data)
        })
    }

</script>
{#if isOpen}
    <div class="modal">
        <div class="contents">

            <h4 class="text-dark"> setup appointment</h4>
            <form on:submit|preventDefault={submit}>
                <div class="mb-3">
                    <label class="form-label">Title</label>
                    <input bind:value={appointment_obj.title} type="text" class="form-control">
                </div>

                <div class="mb-3">
                    <label class="form-label">Description</label>
                    <textarea rows="4" bind:value={appointment_obj.description}  class="form-control"></textarea>
                </div>
                <div class="text-center my-5">
                    <DateInput bind:value={date_time} />
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


</style>
