<script>
    import StudentAdminPanel from "../../../layouts/StudentAdminPanel.svelte";
    import axios from "axios";
    import {baseStudentsUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {toast} from "@zerodevx/svelte-toast";
    import {onMount} from "svelte";
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";

    let passObject = {
        previous_pass: '',
        new_pass: '',
        repeat_pass: '',
    }


    console.log(passObject)

    const updateProfile = () => {
        if (passObject.new_pass !== passObject.repeat_pass) {
            toast.push('password and repeat password are not equal')
            return
        }
        axios({
            url: `${baseStudentsUrl}profile/update/password/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(passObject)
        }).then(r => {
            if (r.status === 202) {
                toast.push('password updated')

                console.log(r.data)
            }
        })


    }

</script>

<ProfessorAdminPanel>
    <div slot="title">
        <h3 class="my-1">Change Password</h3>
    </div>
    <div slot="content">

        <div class="row">
            <div class="col-3"></div>
            <div class="col-6">
                <div class="row">
                    <div class="my-2 col-md-12">
                        <label class="form-label text-white">Previews Password:</label>
                        <input bind:value={passObject.previous_pass} type="password" class="form-control">
                    </div>
                    <div class="my-2 col-md-12">
                        <label class="form-label text-white">New Password:</label>
                        <input bind:value={passObject.new_pass} type="password" class="form-control">
                    </div>
                    <div class="my-2 col-md-12">
                        <label class="form-label text-white">Repeat Password:</label>
                        <input bind:value={passObject.repeat_pass} type="password" class="form-control">
                    </div>
                </div>
            </div>
            <div class="col-3"></div>
        </div>


        <div class="d-grid gap-2 mt-5">
            <button class="btn btn-primary appointment-button" type="submit" on:click={updateProfile}>Save</button>
        </div>
    </div>
</ProfessorAdminPanel>
