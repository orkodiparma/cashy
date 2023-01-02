<script>
export default {
	// Properties returned from data() become reactive state
	// and will be exposed on `this`.
	data() {
		return {
			pricelist: {
				beer: { price: 3, deposit: 1, name: 'Bier', amount: 0 },
				cheapbeer: { price: 1.5, deposit: 1, name: 'Krisenbier', amount: 0 },
				longdrink: { price: 6, deposit: 1, name: 'Longdrink', amount: 0 },
				lemonade: { price: 3, deposit: 1, name: 'Limonade', amount: 0 },
				liqueur: { price: 2, deposit: 0, name: 'Likör', amount: 0 },
				schnapps: { price: 2.5, deposit: 0, name: 'Schnaps', amount: 0 },
				winespritzer: { price: 3.5, deposit: 1, name: 'Weinschorle', amount: 0 },
				water: { price: 1, deposit: 1, name: 'Wasser', amount: 0 },
				depositBack: { price: 0, deposit: -1, name: 'Flasche zurück', amount: 0 }
			}
			// lastInteraction: new Date(),
			// lastAutoReset: new Date(),
			// secondsToResetDisplay: 30
		}
	},
	computed: {
		drinks_price() {
			let total = 0
			for (let [drink, props] of Object.entries(this.pricelist)) {
				total += (this.pricelist[drink].price * this.pricelist[drink].amount)
			}
			return total
		},
		deposit() {
			let total = 0
			for (let [drink, props] of Object.entries(this.pricelist)) {
				total += (this.pricelist[drink].deposit * this.pricelist[drink].amount)
			}
			return total
		},
		total() {
			return this.drinks_price + this.deposit
		},
	},
	// Methods are functions that mutate state and trigger updates.
	// They can be bound as event listeners in templates.
	methods: {
		clear() {
			// this.lastInteraction = new Date()
			for (let [drink, props] of Object.entries(this.pricelist)) {
				this.pricelist[drink].amount = 0
			}
		},
		addDrink(drink) {
			// this.lastInteraction = new Date()
			this.pricelist[drink].amount++
		},
		removeDrink(drink) {
			// this.lastInteraction = new Date()
			if (this.pricelist[drink].amount == 0) {
				return
			}
			this.pricelist[drink].amount--
		}
	},

	// Lifecycle hooks are called at different stages
	// of a component's lifecycle.
	// This function will be called when the component is mounted.
	mounted() {
		// setInterval( () => {
		// 	if ( ((this.lastInteraction - this.lastAutoReset) / 1000) > 30 ) {
		// 		this.clear()
		// 		this.lastAutoReset = new Date()
		// 		this.secondsToResetDisplay = 31
		// 		console.log('auto reset')
		// 	}
		// 	if (this.secondsToResetDisplay > 0) {
		// 		this.secondsToResetDisplay--
		// 	}

		// }, 1*1000)
	}
}
</script>
<template>
	<main>
		<div id="layout">
			<div id="totalbox">
				<div id="total" class="fatlabel highlite">
					<div>Endsumme:</div>
					<div>{{ new Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(total) }}</div>
				</div>
				<div id="depositButtons" class="fatlabel highlite">
					<div v-if="deposit < 0">{{ Math.abs(deposit) }} Pfandmarken nehmen</div>
					<div v-if="deposit >= 0">{{ Math.abs(deposit) }} Pfandmarken geben</div>
				</div>

				<button v-on:click="clear">Clear {{ secondsToResetDisplay }}</button>

			</div>
			<table id="table" border="0">
				<tbody>
					<tr v-for="(value, key) in pricelist" :key="key">
						<td align="center" style="font-weight: bold;"
							:class="{ 'highlite': this.pricelist[key].amount > 0 }">
							{{ this.pricelist[key].amount }}
						</td>
						<td align="center" :class="{ 'highlite': this.pricelist[key].amount > 0 }">{{ value.name }}
						</td>
						<td>
							<button v-on:click="addDrink(key)">+</button>
						</td>
						<td>
							<button v-on:click="removeDrink(key)">-</button>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</main>
</template>

<style scoped>
.highlite {
	color: aqua;
}

#layout {
	display: flex;
	flex-flow: row;
}

#totalbox {
	margin-right: 20px;
	background-color: rgba(255, 255, 255, 0.2);
	border-radius: 5px;
	padding: 10px;
}

#drinkprice,
#deposit,
#total {
	display: flex;
	flex-flow: row;
	justify-content: space-between;
	width: 500px;
}

.fatlabel {
	font-weight: bold;
	font-size: 50px;
}

.fatlabel.light {
	font-size: x-large;
}

#depositButtons.fatlabel {
	font-size: 40px;
}

#table {
	width: 100%;
	font-size: x-large;
}

button {
	font-size: xx-large;
	line-height: 50px;
	width: 100px;
}
</style>
