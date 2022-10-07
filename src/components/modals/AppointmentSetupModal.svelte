<script>
    import axios from "axios";
    import {baseProfsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {TimePicker, TimePickerModal} from "svelte-time-picker";
    import {DateInput} from "date-picker-svelte";
    import {closeModal} from "svelte-modals";
    import {toast} from "@zerodevx/svelte-toast";

    export let isOpen
    export let appointment

    let date_time = new Date()
    let appointmentObject = {
        time: Math.round(date_time.getTime()/1000),
        appointment_id:appointment.id
    }
    const setUp = () => {
        axios({
            url: `${baseProfsUrl}appointments/setup/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(appointmentObject)
        }).then(r => {
            if (r.status === 200) {
                toast.push('appointment setted up')
                closeModal()
            }
            console.log(r.data)
        })
    }



</script>
{#if isOpen}
    <div class="modal">
        <div class="contents">
            <h4 class="text-black">
                pick a time:
            </h4>
            <div class="text-center my-5">
                <DateInput bind:value={date_time} />
            </div>
            <button on:click={setUp} class="appointment-button">
                SetUp Appointment
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
