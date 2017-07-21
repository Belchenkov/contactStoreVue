<template>
    <div>
        <h1 class="page-header">Contacts</h1>
        <form action="#" @submit.prevent="edit ? updateContact(contact.id) : createContact()">
            <div class="form-group">
                <label for="name">Name</label>
                <input type="text" name="name" v-model="contact.name" id="name" class="form-control">
            </div>
            <div class="form-group">
                <label for="email">Email</label>
                <input type="email" name="email" v-model="contact.email" id="email" class="form-control">
            </div>
            <div class="form-group">
                <label for="phone">Phone</label>
                <input type="phone" name="phone" v-model="contact.phone" id="phone" class="form-control">
            </div>
            <div class="form-group">
                <button v-show="!edit" type="submit" class="btn btn-primary">New Contact</button>
                <button v-show="edit" type="submit" class="btn btn-warning">Update Contact</button>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                edit:true,
                list: [],
                contact: {
                    id: '',
                    name: '',
                    email: '',
                    phone: ''
                }
            }
        },
        mounted: function () {
            console.log('Contacts Component Loaded');
        },
        methods: {
            createContact: function () {
                console.log('Creating contact ...');
                return;
            },
            updateContact: function (id) {
                console.log('Updating contact ' + id + ' ...');
                return;
            }
        }
    }
</script>