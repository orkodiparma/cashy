<script>
export default {
	data() {
		return {
			drinksMenu: {
				depositBack: { price: 0, deposit: -1, name: 'Flasche zurück', amount: 0 },
				beer: { price: 3, deposit: 1, name: 'Bier', amount: 0 },
				cheapbeer: { price: 1.5, deposit: 1, name: 'Krisenbier', amount: 0 },
				longdrink: { price: 6, deposit: 1, name: 'Longdrink', amount: 0 },
				lemonade: { price: 3, deposit: 1, name: 'Limonade', amount: 0 },
				liqueur: { price: 2, deposit: 0, name: 'Likör', amount: 0 },
				schnapps: { price: 2.5, deposit: 0, name: 'Schnaps', amount: 0 },
				winespritzer: { price: 3.5, deposit: 1, name: 'Weinschorle', amount: 0 },
				water: { price: 1, deposit: 1, name: 'Wasser', amount: 0 }
			}
		}
	},
	computed: {
		drinksPrice() {
			let total = 0
			for (let [_drinkID, menuItem] of Object.entries(this.drinksMenu)) {
				total += menuItem.price * menuItem.amount
			}
			return total
		},
		deposit() {
			let total = 0
			for (let [_drinkID, menuItem] of Object.entries(this.drinksMenu)) {
				total += (menuItem.deposit * menuItem.amount)
			}
			return total
		},
		total() {
			return this.drinksPrice + this.deposit
		},
	},
	methods: {
		clear() {
			for (let [_drinkID, menuItem] of Object.entries(this.drinksMenu)) {
				menuItem.amount = 0
			}
		},
		addDrink(drinkID) {
			this.drinksMenu[drinkID].amount++
		},
		removeDrink(drinkID) {
			if (this.drinksMenu[drinkID].amount == 0) {
				return
			}
			this.drinksMenu[drinkID].amount--
		}
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
				<button v-on:click="clear">Clear</button>
			</div>
			<table id="table" border="0">
				<tbody>
					<tr v-for="(menuItem, key) in drinksMenu" :key="key">
						<td align="center" style="font-weight: bold;"
							:class="{ 'highlite': menuItem.amount > 0 }">
							{{ menuItem.amount }}
						</td>
						<td align="center" :class="{ 'highlite': menuItem.amount > 0 }">{{ menuItem.name }}
						</td>
						<td>
							<button v-on:click="removeDrink(key)" :disabled="menuItem.amount < 1">-</button>
						</td>
						<td>
							<button v-on:click="addDrink(key)">+</button>
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
	align-items: flex-start;
}
@media (max-width: 930px) {
	#layout {
		flex-flow: column;
	}
}
#totalbox {
	margin-right: 20px;
	background-color: rgba(255, 255, 255, 0.2);
	border-radius: 5px;
	padding: 10px;
}
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
	font-size: x-large;
}
button {
	font-size: xx-large;
	line-height: 50px;
	width: 100px;
}
@media (max-width: 530px) {
	#table {
		padding-top: 15px;
	}
	#total, #table {
		width: 90vw;
		margin: 0 auto;
	}
	.fatlabel {
		font-weight: bold;
		font-size: xx-large;
	}
	#depositButtons.fatlabel {
		font-size: x-large;
	}
}
</style>
