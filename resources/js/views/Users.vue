<template>
    <div>
        <div v-if="loading"><p>Loading...</p></div>

        <div v-if="error">{{error}}</div>

        <ul v-if="users">
            <li v-for="{name, email} in users">
                <strong>Name:</strong> {{name}},
                <strong>Email:</strong> {{email}}
            </li>
        </ul>
    </div>
</template>

<script>
    import axios from 'axios';

    const getUsers = (page, callback) => {
        const params = {page};
        axios
            .get('/api/users', {params})
            .then(response => {
                callback(null, response.data);
            }).catch(error => {
            callback(error, error.response.data);
        });
    };

    export default {
        name: "Users",
        data() {
            return {
                users: null,
                error: null,
                links: {
                    first: null,
                    last: null,
                    prev: null,
                    next: null
                },
                meta: null
            }
        },
        created() {

        },
        methods: {
            setData(error, {data: users, links, meta}) {
                if (error) {
                    this.error = error.toString();
                } else {
                    this.users = users;
                    this.links = links;
                    this.meta = meta;
                }
            }
        },
        beforeRouteEnter (to, from, next) {
            getUsers(to.query.page, (error, data)=>{
                next(vm=> vm.setData(error, data));
            })
        }
    }
</script>

<style scoped>

</style>