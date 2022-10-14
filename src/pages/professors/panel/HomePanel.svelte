<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import {userD} from "../../../utils/auth.js";
    import axios from "axios";
    import {baseProfsUrl, baseStudentsUrl, baseUrl} from "../../../utils/consts.js";
    import {toast} from "@zerodevx/svelte-toast";
    import {navigate} from "svelte-navigator";
    import {openModal} from "svelte-modals";
    import AddContactModal from "../../../components/modals/AddContactModal.svelte";
    import {onMount} from "svelte";

    let userObject = {
        name: '',
        family: '',
        email: '',
        phone: '',
        bio: '',
    }

    let contacts = []
    let locations = []


    userD.subscribe(v => {
        userObject = {
            name: v.user.name,
            family: v.user.family,
            email: v.user.email,
            phone: v.user.phone,
            bio: v.user.bio,
        }
    })

    onMount(() => {
        getContacts()
        getLocations()
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
    const getContacts = () => {
        axios({
            url: `${baseProfsUrl}contacts/all/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            contacts = r.data
            console.log(r)
        })
    }
    const getLocations = () => {
        axios({
            url: `${baseProfsUrl}locations/all/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            locations = r.data
            console.log(r)
        })
    }
    let avatar, fileinput;

    console.log(userObject)
    const onFileSelected = async (e) => {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);

        reader.onload = async e => {
            avatar = e.target.result


            axios({
                url: `${baseStudentsUrl}profile/update/avatar/`,
                method: 'post',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + $userD.access
                },
                data: JSON.stringify({avatar: avatar})
            }).then(r => {
                if (r.status === 200) {
                    toast.push('avatar updated')
                    $userD.user = r.data
                }
                console.log(r.data)
            })

        };
    }
    const setLocation = (location_id) => {
        axios({
            url: `${baseProfsUrl}profile/update/location/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify({
                location_id: location_id
            })
        }).then(r => {
            if (r.status === 200) {
                toast.push('location changed')
                $userD.user.location
            }
            console.log(r.data)
        })
    }
</script>

<ProfessorAdminPanel>
    <div slot="title">
        <h3 class="my-1">personal information</h3>
    </div>
    <div slot="content">
        <h3>set Your Location</h3>
        <div class="row">
            {#each locations as location}
                <div class="col-md-3 ">
                    <div class=" card m-1 h-100">
                        <div class="text-center">
                            <img src={baseUrl+location.icon} class=" contact-icon" alt="...">
                        </div>
                        <div class="card-body ">
                            <h5 class="card-title text-center my-2">{location.name}</h5>
                            <a class="btn btn-primary w-100" on:click={()=>{
                                setLocation(location.id)
                            }}>set</a>
                        </div>

                    </div>
                </div>
            {/each}
        </div>

        <div class="d-flex justify-content-center">
            <input style="display: none" name="avatar" bind:this={fileinput} type="file" accept=".jpg, .jpeg, .png"
                   on:change={(e)=>onFileSelected(e)}>
            {#if avatar}
                <img src={avatar} alt=""
                     class="rounded-circle profile-edit-image m-5" on:click={()=>{
                     fileinput.click()
                 }}>
            {:else }
                {#if $userD.user.avatar}
                    <img src={baseUrl+$userD.user.avatar} alt=""
                         class="rounded-circle profile-edit-image m-5" on:click={()=>{
                     fileinput.click()
                 }}>
                {:else }
                    <img src="https://www.nicepng.com/png/detail/73-730154_open-default-profile-picture-png.png" alt=""
                         class="rounded-circle profile-edit-image m-5" on:click={()=>{
                     fileinput.click()
                 }}>
                {/if}
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

        <hr>

        <div class="d-flex justify-content-between">
            <p class="my-4">Your Contact List</p>
            <button class="btn appointment-button" on:click={()=>{
                openModal(AddContactModal,{})
            }}>
                add +
            </button>
        </div>

        <div class="row">
            {#each contacts as contact}
                <div class="col-md-3 ">
                    <div class=" card m-1 h-100">
                        <div class="text-center">
                            <img src={baseUrl+contact.type.icon} class=" contact-icon" alt="...">
                        </div>
                        <div class="card-body ">
                            <h5 class="card-title text-center">{contact.name}</h5>
                            <p class="card-text text-center">{contact.value}</p>
                            <a href={contact.value} class="btn btn-primary w-100">Go Check</a>
                        </div>

                    </div>
                </div>

            {/each}
        </div>
    </div>
</ProfessorAdminPanel>
