
<template>
    <v-container>
        <v-layout justify-center v-show="!isEditing">
            <v-card min-width="400">
                <v-card-title>{{task.title}}</v-card-title>
                <v-card-text>
                    {{task.detail}}
                </v-card-text>
                <v-card-actions>
                    <v-btn color="warning" text v-show="!task.done" @click="completeTask(task)">Progress</v-btn>
                    <v-btn color="success" text v-show="task.done">Completed</v-btn>
                    <v-spacer></v-spacer>
                    <v-btn icon @click="showForm">
                        <v-icon>mdi-pencil</v-icon>
                    </v-btn>
                    <v-btn icon @click="deleteTask(task)">
                        <v-icon>mdi-delete</v-icon>
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-layout>
        <v-layout justify-center v-show="isEditing">
            <v-card min-width="400">
                <v-card-text>
                    <v-text-field
                        placeholder="Title"
                        v-model="task.title"
                    ></v-text-field>
                    <v-textarea
                        hint='Detail'
                        rows=1
                        auto-grow
                        v-model="task.detail">
                    </v-textarea>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary" text @click="hideForm">Save</v-btn>
                </v-card-actions>
            </v-card>
        </v-layout>
    </v-container>
</template>

<script>
    export default {
        name: 'TaskComponent',
        props: {
            task: {
                type: Object,
            },
        },
        data() {
            return {
                isEditing: false
            }
        },
        methods: {
            deleteTask(task) {
                this.$emit('delete-task', task)
            },
            showForm() {
                this.isEditing = true
            },
            hideForm() {
                this.isEditing = false
            },
            completeTask(task) {
                this.$emit('complete-task', task)
            }
        },
    }
</script>

<style scoped>

</style>