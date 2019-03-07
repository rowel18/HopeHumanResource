<style>
.widget-user-header{
    background-position: center center;
    background-size: cover;
}
</style>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-3">
                <div class="card card-widget widget-user">
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header text-white" style="background: url('./img/wallpaper.png')">
                        <h3 class="widget-user-username">John Doe</h3>
                        <h5 class="widget-user-desc">Web Designer</h5>
                    </div>
                    <div class="widget-user-image">
                        <img class="img-circle" src="" alt="User Avatar">
                    </div>
                    <div class="card-footer">
                        <div class="row">
                        <div class="col-sm-4 border-right">
                            <div class="description-block">
                            <h5 class="description-header">3,200</h5>
                            <span class="description-text">SALES</span>
                            </div>
                            <!-- Description-block -->
                        </div>
                        <!-- Col -->
                        <div class="col-sm-4 border-right">
                            <div class="description-block">
                            <h5 class="description-header">13,000</h5>
                            <span class="description-text">FOLLOWERS</span>
                            </div>
                            <!-- Description-block -->
                        </div>
                        <!-- Col -->
                        <div class="col-sm-4">
                            <div class="description-block">
                            <h5 class="description-header">35</h5>
                            <span class="description-text">PRODUCTS</span>
                            </div>
                            <!-- Description-block -->
                        </div>
                        <!-- Col -->
                        </div>
                        <!-- Row -->
                    </div>
                </div>
            </div>

            <div class="col-md-12">
                <div class="card">
                    <!-- Card-Header -->
                    <div class="card-header p-2">
                        <ul class="nav nav-pills">
                        <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                        <li class="nav-item"><a class="nav-link active show" href="#settings" data-toggle="tab">Settings</a></li>
                        </ul>
                    </div>
                    <!-- Card-Header -->
                    <div class="card-body">
                        <div class="tab-content">
                        <div class="tab-pane" id="activity">
                            <h1>Display User Activity</h1>
                        </div>
                        <!-- Tab-pane -->

                        <div class="tab-pane active show" id="settings">
                            <form class="form-horizontal">
                            <div class="form-group">
                                <label for="inputName" class="col-sm-12 control-label">Name</label>
                                <div class="col-sm-12">
                                <input type="text" v-model="form.name" class="form-control" id="inputName" placeholder="Name">
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="inputEmail" class="col-sm-12 control-label">Email</label>
                                <div class="col-sm-12">
                                <input type="email" v-model="form.email" class="form-control" id="inputEmail" placeholder="Email">
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="inputExperience" class="col-sm-12 control-label">Experience</label>
                                <div class="col-sm-12">
                                <textarea v-model="form.bio" class="form-control" id="inputExperience" placeholder="Experience"></textarea>
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="inputPhoto" class="col-sm-12 control-label">Profile Photo</label>
                                <div class="col-sm-12">
                                <input type="file" @change="updateProfile" class="form-control-file-border" id="inputPhoto">
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="inputPassport" class="col-sm-12 control-label">Passport (leave empty if not changing)</label>
                                <div class="col-sm-12">
                                <input type="text" v-model="form.password" class="form-control" id="inputPassport" placeholder="Passport">
                                </div>
                            </div>
                            <div class="form-group">
                                <div class="col-sm-offset-2 col-sm-10">
                                <button @click.prevent="updateInfo" type="submit" class="btn btn-success">Update</button>
                                </div>
                            </div>
                            </form>
                        </div>
                        <!-- Tab-pane -->
                        </div>
                        <!-- Tab-content -->
                    </div>
                    <!-- Card-body -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                form: new Form({
                    id: '',
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                })
            }
        },

        mounted() {
            console.log('Component mounted.')
        },

        methods: {
            updateInfo() {
                this.$Progress.start();
                this.form.put('api/profile').then(()=>{
                    
                    this.$Progress.finish();
                }).catch(()=>{
                    this.$Progress.fail();
                });
            },

            updateProfile(e) {
                // console.log('UPLOADING!!!')
                let file = e.target.files[0];
                console.log(file);
                let reader = new FileReader();
                if(file['size'] < 2111775) {
                    reader.onloadend = (file) => {
                        // console.log('RESULT', reader.result);
                        this.form.photo = reader.result;
                    }
                    reader.readAsDataURL(file);
                }
                else {
                    swal.fire({
                        type: 'error',
                        title: 'Ooops....',
                        text: 'You are uploading large file!',
                    })
                }
            }
        },

        created() {
            axios.get("api/profile").then( ({ data }) => (this.form.fill(data)));
        }
    }
</script>
