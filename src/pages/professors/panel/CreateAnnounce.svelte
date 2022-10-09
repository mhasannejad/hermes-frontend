<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import axios from "axios";
    import {baseProfsUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {toast} from "@zerodevx/svelte-toast";
    import {navigate} from "svelte-navigator";


    let avatar, fileinput;
    let announceObj = {
        title: '',
        body: '',
        cover: '',
        cover_ext: 'jpeg',
        skills:['programming','pharmacy'].join(',')
    }
    const onFileSelected = (e) => {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = e => {
            avatar = e.target.result
            announceObj.cover = avatar
            console.log(avatar)
        };
    }

    const submitAnnouncement = () => {
        axios({
            url: `${baseProfsUrl}announces/add/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(announceObj)
        }).then(r => {
            if (r.status === 200) {
                navigate('/panel/professor/announces')
            }
            console.log(r.data)
        })
    }
</script>

<ProfessorAdminPanel>
    <div slot="title">
        Create New Announcement
    </div>
    <div slot="content">
        <input style="display:none" type="file" accept=".jpg, .jpeg, .png" on:change={(e)=>onFileSelected(e)}
               bind:this={fileinput}>
        <div class="image-drop-zone text-center" on:click={()=>{fileinput.click()}}>

            {#if avatar}
                <img src={avatar} alt="">
            {:else }
                <p>Drop Cover Image Here</p>
            {/if}
        </div>

        <div class="mb-3">
            <label class="form-label text-white">Title</label>
            <input type="text" class="form-control" placeholder="Thesis Subject" bind:value={announceObj.title}>
        </div>
        <div class="mb-3">
            <label class="form-label text-white">About Announcement</label>
            <textarea class="form-control" rows="6" bind:value={announceObj.body}></textarea>
        </div>

        <button class="appointment-button btn w-100" on:click={submitAnnouncement}>
            Submit
        </button>
    </div>
</ProfessorAdminPanel>
