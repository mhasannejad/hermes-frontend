<script>
    import svelteLogo from './assets/svelte.svg'
    import Counter from './lib/Counter.svelte'
    import {Link, navigate, Route, Router} from "svelte-navigator";
    import StudentsHomePage from "./pages/students/StudentsHomePage.svelte";
    import {closeModal, Modals, openModal} from "svelte-modals";
    import LoginModal from "./components/modals/LoginModal.svelte";
    import RegisterModal from "./components/modals/RegisterModal.svelte";
    import {userD} from "./utils/auth.js";
    import ProfessorsList from "./pages/students/ProfessorsList.svelte";
    import ProfessorProfile from "./pages/students/ProfessorProfile.svelte";
    import PostView from "./pages/students/PostView.svelte";
    import HomePanel from "./pages/professors/panel/HomePanel.svelte";
    import ProfileEdit from "./pages/professors/panel/ProfileEdit.svelte";
    import Announces from "./pages/professors/panel/Announces.svelte";
    import Posts from "./pages/professors/panel/Posts.svelte";
    import Appointments from "./pages/professors/panel/Appointments.svelte";
    import References from "./pages/professors/panel/References.svelte";
    import MainPanel from "./pages/students/panel/MainPanel.svelte";
    import {SvelteToast} from '@zerodevx/svelte-toast'
    import ChangePassword from "./pages/students/panel/ChangePassword.svelte";
    import AllUserAppointments from "./pages/students/panel/AllUserAppointments.svelte";
    import ProfessorChangePass from "./pages/professors/panel/ProfessorChangePass.svelte";
    import CreateAnnounce from "./pages/professors/panel/CreateAnnounce.svelte";
    import AnnounceItem from "./pages/professors/panel/AnnounceItem.svelte";

    const navigateToPanel = () => {
        if ($userD.user.mode === 2) {
            navigate('/panel/professor/')
        } else {
            navigate('/panel/student/')
        }
    }
    const options = {}
</script>
<SvelteToast {options}/>
<Router>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container-fluid">
            <Link class="navbar-brand" to="/">Schooler</Link>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
                    aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <Link class="nav-link" to="/">Home</Link>
                    </li>
                    <li class="nav-item">
                        <Link class="nav-link" to="/professors">Professors</Link>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link">Students</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link ">Circles</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link ">Posts</a>
                    </li>
                </ul>

                <div class="" style="width: 350px;margin-right: 25px;margin-left: 25px">
                    {#if !$userD.access}
                        <div class="row">
                            <button class="btn btn-outline-success  col-6" type="submit" on:click={()=>{
                        openModal(LoginModal)
                    }}>Login
                            </button>
                            <button class="btn btn-outline-success col-6" type="submit" on:click={()=>{
                        openModal(RegisterModal)
                    }}>Register
                            </button>
                        </div>
                    {:else }
                        <div class="row">
                            <button class="btn btn-outline-success col-6" type="submit" on:click={navigateToPanel}>
                                Profile
                            </button>
                            <button class="btn btn-outline-success col-6" type="submit" on:click={()=>{
                        navigate('/')
                        $userD = {}

                    }}>Logout
                            </button>
                        </div>
                    {/if}
                </div>


            </div>
        </div>
    </nav>
    <Route path="/">
        <StudentsHomePage/>
    </Route>
    <Route path="/professors">
        <ProfessorsList/>
    </Route>
    <Route path="/professor/:id" let:params>
        <ProfessorProfile prof_id={params.id}/>
    </Route>
    <Route path="/post/:id" let:params>
        <PostView id={params.id}/>
    </Route>
    <Route path="/panel/professor/*">
        <Route path="/">
            <HomePanel/>
        </Route>
        <Route path="/profile">
            <ProfileEdit/>
        </Route>
        <Route path="/password">
            <ProfessorChangePass/>
        </Route>
        <Route path="/announces">
            <Announces/>
        </Route>
        <Route path="/announces/:id" let:params >
            <AnnounceItem id={params.id} />
        </Route>
        <Route path="/announces/create">
            <CreateAnnounce />
        </Route>
        <Route path="/references">
            <References/>
        </Route>
        <Route path="/posts">
            <Posts/>
        </Route>
        <Route path="/appointments">
            <Appointments/>
        </Route>
    </Route>
    <Route path="/panel/student/*">
        <Route path="/">
            <MainPanel/>
        </Route>
        <Route path="/password">
            <ChangePassword/>
        </Route>
        <Route path="/appointments">
            <AllUserAppointments/>
        </Route>
    </Route>
</Router>
<Modals>
    <div
            slot="backdrop"
            class="backdrop"
            on:click={closeModal}
    ></div>
</Modals>
<style>
    .backdrop {
        position: fixed;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
        background: rgba(0, 0, 0, 0.50)
    }
</style>
