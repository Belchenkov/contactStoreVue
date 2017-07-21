<template>
    <div>
        <h1 class="page-header">Add Contact</h1>
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

        <h1 class="page-header">Contacts</h1>
        <ul class="list-group">
            <li v-for="contact in list" class="list-group-item">
                <strong>{{ contact.name }}</strong>&nbsp;&nbsp;
                    <span class="label label-info">{{ contact.email }}</span>&nbsp;&nbsp;
                    <span class="label label-warning">{{ contact.phone }}</span><br><br>
                <button class="btn btn-warning btn-xs" @click="showContact(contact.id)">Edit</button>&nbsp;&nbsp;
                <button class="btn btn-danger btn-xs" @click="deleteContact(contact.id)">Delete</button>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                edit:false,
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
            this.fetchContactList();
        },
        methods: {
            fetchContactList: function () {
                console.log('Fetch Contacts ...');
                axios.get('api/contacts')
                    .then( (response) => {
                    console.log(response.data);
                    this.list = response.data;
                })
                .catch( (error) => {
                    console.log(error);
                });

            },
            createContact: function () {
                console.log('Creating contact ...');

                let self = this;
                let params = Object.assign({}, self.contact);
                axios.post('api/contact/store', params)
                    .then(function () {
                        self.contact.name = '';
                        self.contact.email = '';
                        self.contact.phone = '';
                        self.edit = false;
                        self.fetchContactList();
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            showContact(id) {
                let self = this;
                axios.get('api/contact/' + id)
                    .then(function (response) {
                        self.contact.id = response.data.id;
                        self.contact.name = response.data.name;
                        self.contact.email = response.data.email;
                        self.contact.phone = response.data.phone;
                    });
                self.edit = true;
            },

            updateContact: function (id) {
                console.log('Updating contact ' + id + ' ...');

                let self = this;
                let params = Object.assign({}, self.contact);
                axios.patch('api/contact/' + id, params)
                    .then(function () {
                        self.contact.name = '';
                        self.contact.email = '';
                        self.contact.phone = '';
                        self.edit = false;
                        self.fetchContactList();
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            deleteContact: function (id) {
                let self = this;
                axios.delete('api/contact/' + id)
                    .then(function(response) {
                        self.fetchContactList();
                    })
                    .catch(function (error) {
                        console.log(error);
                    })
            }
        }
    }
</script>