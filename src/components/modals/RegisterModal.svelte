<script>
    import axios from "axios";
    import {baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {closeModal} from "svelte-modals";

    export let isOpen
    let register_object = {
        email: '',
        password: '',
        password_r: ''
    }
    const loginF = () => {
        axios({
            url: `${baseUrl}/auth/register/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json'
            },
            data: JSON.stringify(register_object)
        }).then(r => {
            if (r.status === 200) {
                $userD = r.data
                closeModal()
            }
            console.log(r.data)
        })
    }

</script>
{#if isOpen}
    <div class="modal">
        <div class="contents">
            <form on:submit|preventDefault={loginF}>
                <div class="mb-3">
                    <label class="form-label">Email address</label>
                    <input bind:value={register_object.email} type="email" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label">Password</label>
                    <input bind:value={register_object.password} type="password" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label">Password</label>
                    <input bind:value={register_object.password} type="password" class="form-control">
                </div>
                <button type="submit" class="btn btn-primary w-100">Register</button>
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
