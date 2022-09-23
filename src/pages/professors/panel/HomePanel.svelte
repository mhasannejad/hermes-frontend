<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import {userD} from "../../../utils/auth.js";
    import axios from "axios";
    import {baseStudentsUrl} from "../../../utils/consts.js";
    import {toast} from "@zerodevx/svelte-toast";

    let userObject = {
        name: '',
        family: '',
        email: '',
        phone: '',
        bio: '',
    }

    userD.subscribe(v => {
        userObject = {
            name: v.user.name,
            family: v.user.family,
            email: v.user.email,
            phone: v.user.phone,
            bio: v.user.bio,
        }
    })
    const updateProfile = () => {
        axios({
            url: `${baseStudentsUrl}profile/update/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(userObject)
        }).then(r => {
            if (r.status === 200) {
                toast.push('profile updated')
                $userD.user = r.data
                console.log(r.data)
            }
        })


    }

    let avatar, fileinput;

    console.log(userObject)
    const onFileSelected = () => {

    }
</script>

<ProfessorAdminPanel>
    <div slot="title">
        <h3 class="my-1">personal information</h3>
    </div>
    <div slot="content">
        <div class="d-flex justify-content-center">
            <input style="display: none" name="avatar" bind:this={fileinput} type="file" accept=".jpg, .jpeg, .png"
                   on:change={(e)=>onFileSelected(e)}>
            {#if avatar}
                <img src={avatar} alt=""
                     class="rounded-circle profile-edit-image m-5" on:click={()=>{
                     fileinput.click()
                 }}>
            {:else }
                <img src="https://gdb.rferl.org/FD93574C-4AF6-4774-928A-B94D300111A5_w408_r1_s.jpg" alt=""
                     class="rounded-circle profile-edit-image m-5" on:click={()=>{
                     fileinput.click()
                 }}>
            {/if}
        </div>
        <div class="row">
            <div class="my-2 col-md-6">
                <label class="form-label text-white">First Name:</label>
                <input bind:value={userObject.name} type="text" class="form-control"
                       aria-describedby="emailHelp">
            </div>
            <div class="my-2 col-md-6">
                <label class="form-label text-white">Family Name:</label>
                <input bind:value={userObject.family} type="text" class="form-control"
                       aria-describedby="emailHelp">
            </div>
        </div>
        <div class="row">
            <div class="my-2  col-md-6">
                <label class="form-label text-white">Email address:</label>
                <input bind:value={userObject.email} type="email" class="form-control" aria-describedby="emailHelp">
            </div>
            <div class="my-2  col-md-6">
                <label class="form-label text-white">Phone:</label>
                <input bind:value={userObject.phone} type="tel" class="form-control" aria-describedby="emailHelp">
            </div>
        </div>


        <div class="my-2">
            <label class="form-label text-white">Bio:</label>
            <textarea bind:value={userObject.bio} class="form-control" aria-describedby="emailHelp"></textarea>
        </div>
        <div class="d-grid gap-2 mt-5">
            <button class="btn btn-primary appointment-button" type="submit" on:click={updateProfile}>Save</button>
        </div>
    </div>
</ProfessorAdminPanel>
