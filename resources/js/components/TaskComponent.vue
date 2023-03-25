<template>
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header bg-dark">
                    <div class="row">
                        <div class="col-md-6">
                            <h4 class="text-light float-start">{{ title }}</h4>
                        </div>
                        <div class="col-md-6">
                            <button  @click="createTask" class="btn btn-success btn-sm float-end">
                                Add Task
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal -->
    <div
        class="modal fade"
        id="taskModal"
        tabindex="-1"
        aria-labelledby="taskModalLabel"
        aria-hidden="true"
    >
        <div class="modal-dialog modal-dialog-centered modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="taskModalLabel">
                        Add New Task
                    </h5>
                    <button
                        type="button"
                        class="btn-close"
                        data-bs-dismiss="modal"
                        aria-label="Close"
                    ></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-4">
                            <div class="form-group">
                                <label for="title">Title</label>
                                <input type="text" class="form-control" v-model="taskData.title">
                                <span class="text-danger" v-show="taskErrors.title">Title Empty</span>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="form-group">
                                <label for="date">Date</label>
                                <input type="date" class="form-control" v-model="taskData.date">
                                <span class="text-danger" v-show="taskErrors.date">Date Empty</span>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="form-group">
                                <label for="time">Time</label>
                                <input type="time" class="form-control" v-model="taskData.time">
                                <span class="text-danger" v-show="taskErrors.time">Time Empty</span>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12">
                            <div class="form-group">
                            <label for="detail">Details</label>
                            <textarea class="form-control" rows="3" v-model="taskData.detail"></textarea>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button
                        type="button"
                        class="btn btn-secondary"
                        data-bs-dismiss="modal"
                    >
                        Close
                    </button>
                    <button type="button" @click="storeTask" class="btn btn-primary">
                        Add Task
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    setup: () => ({
        title: "All Tasks",
    }),
    data() {
        return {
            taskData: {
                id: '',
                title: '',
                date: '',
                time: '',
                detail: '',
            },
            taskErrors: {
                title: false,
                date: false,
                time: false,
            }
        }
    },
    methods: {
        createTask() {
            $('#taskModal').modal('show')
        },

        storeTask() {
            this.taskData.title == '' ? this.taskErrors.title = true : this.taskErrors.title = false
            this.taskData.date == '' ? this.taskErrors.date = true : this.taskErrors.date = false
            this.taskData.time == '' ? this.taskErrors.time = true : this.taskErrors.time = false
            if(this.taskData.title && this.taskData.date && this.taskData.time) {
                axios.post(window.url + 'api/storeTask', this.taskData).then(response => {
                    this.getTasks()
                }).catch(errors => {
                    console.log(errors)
                }).finally(() => {
                    $('#taskModal').modal('hide')
                });
            }
        }
    }
}
</script>
