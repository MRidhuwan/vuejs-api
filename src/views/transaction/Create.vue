<template>
	<div class="container my-5">
		<div class="row justify-content-center">
			<div class="col-8">
				<router-link
					:to="{ name: 'transaction.index' }"
					class="btn btn-primary btn-sm rounded shadow mb-3"
				>
					Back
				</router-link>

				<div class="card rounded shadow">
					<div class="card-header">Transaction Create</div>

					<div class="card-body">
						<form @submit.prevent="store()">
							<div class="mb-3">
								<label for="" class="form-label">Name</label>
								<input
									type="text"
									class="form-control"
									v-model="transaction.name"
								/>
								<div v-if="validation.name" class="text-danger">
									{{ validation.name[0] }}
								</div>
							</div>

							<div class="mb-3">
								<label for="" class="form-label">Qty</label>
								<input
									type="text"
									class="form-control"
									v-model="transaction.qty"
								/>
								<div v-if="validation.qty" class="text-danger">
									{{ validation.qty[0] }}
								</div>
							</div>

							<div class="mb-3">
								<label for="" class="form-label">Harga</label>
								<input
									type="text"
									class="form-control"
									v-model="transaction.harga"
								/>
								<div v-if="validation.harga" class="text-danger">
									{{ validation.harga[0] }}
								</div>
							</div>
							<button class="btn btn-outline-primary">Submit</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

export default {
	setup() {
		//data binding
		const transaction = reactive({
			name: '',
			qty: '',
			harga: '',
		});

		const validation = ref([]);
		const router = useRouter();

		function store() {
			axios
				.post('http://127.0.0.1:8000/api/transaction', transaction)
				.then(() => {
					router.push({
						name: 'transaction.index',
					});
				})
				.catch((error) => {
					validation.value = error.response.data;
				});
		}

		return {
			transaction,
			validation,
			router,
			store,
		};
	},
};
</script>
