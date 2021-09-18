<template>
   <div>
       <table class="table">
        <thead>
            <tr>
                <th>
                    Title
                </th>
                <th>
                    Post text
                </th>
                <th>
                    Created date
                </th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="post in posts.data" :key="post.id">
                <td>{{ post.title }}</td>
                <td>{{ post.post_text.substring(0, 50)}}</td>
                <td>{{ post.created_at }}</td>
                <td></td>
            </tr>
        </tbody>
    </table>  
    <pagination :data="posts" @pagination-change-page="getResults"></pagination>
   </div>
</template>

<script>
    export default {
        data() {
            return {
                posts: {},
            }
        },

        mounted() {
            this.getResults();
        },

        methods: {
		// Our method to GET results from a Laravel endpoint
		getResults(page = 1) {
			axios.get('/api/posts?page=' + page)
				.then(response => {
					this.posts = response.data;
				});
		}
	}

    }
</script>
