<template>
	<div
		v-if="collapsed"
	>
		<div class="vertical-bar">
			<span class="vertical-unit-min">{{min}}</span>
			<span class="vertical-unit-max">{{max}}</span>

			<div class="vertical-scale">
				<v-progress-linear
					:value="getValue(value, min, max)"
					height="10"
					rounded
					background-color="#4a4a4a"
					:color="getColor(value,optimum)"
					:title="getCollapsedTitle(metric, value, optimum, min, max)"
				/>
			</div>

			<div
				class="vertical-marker"
				v-bind:style="{marginLeft: `20%`}"
			>
				<i class="icon-triangle-marker" title="title"></i>

			</div>
		</div>
	</div>

	<div 
		v-else
	>
		<div class="bar">
			<span class="unit-min">{{min}}</span>
			<span class="unit-max">{{max}}</span>

			<div class="scale">
				<v-progress-linear
					:value="getValue(value, min, max)"
					height="5"
					rounded
					background-color="#4a4a4a"
					:color="getColor(value,optimum)"
					:title="metric + ': ' + getTitle(value, min, max)"
				/>
			</div>

			<div
				class="marker"
				v-bind:style="{marginLeft: `${getValue(optimum, min, max)}%`}"
			>
				<i class="icon-triangle-marker" :style="markerStyles"></i>
			</div>
		</div>
	</div>
</template>

<script>

	
	export default{


		props: {
			value: Number,
			min: Number,
			max: Number,
			optimum: Number,
			metric: String,
			collapsed: Boolean,
		},

		methods : {
			getValue(value, min, max) {

				var usageValue;

				if (min < max) {

					usageValue = (value/max) * 100;
					return usageValue;
				}

				if (min > max) {

					usageValue =  (value/min) *100;
					return usageValue;
				}
			},

			getOptimum(optimum, min, max) {

				var optimumValue;

				if (min < max) {

					optimumValue = (optimum/max) * 100;
					return optimumValue;
				}

				if (min > max) {

					optimumValue =  (optimum/min) *100;
					return optimumValue;
				}
			},

			getTitle(value, min, max){

				var title;

				if (min < max) {

					title = value;
					return title;
				}

				if (min > max) {

					title =  value * -1;
					return title;
				}
			},

			getColor(value, optimum){

				var color;

				if (value < optimum) {

					color = "#29d1b3";
					return color;
				}

				if (value > optimum) {

					color = "#fa3861";
					return color;
				}
			},

			getCollapsedTitle(metric, value, optimum, min, max) {

				var title;

				if (min < max) {

					title = metric + ': ' + ' value(' + value + ')' + ' optimum(' + optimum + ')';
					return title;
				}

				if (min > max) {

					title = metric + ': ' + ' value(' + value * -1 + ')' + ' optimum(' + optimum * -1 + ')';
					return title;
				}
			}		
		},

		computed: {
			markerStyles() {

				var color;

				if (this.value < this.optimum) {
					color = "#29d1b3";
				}

				else if (this.value > this.optimum) {
					color = "#fa3861";	
				}

				return {
					color: color,
				};
			},
		},
	}
</script>


<style scoped>

	.bar {
		padding: 25px 0;
		position: relative;
		margin: 0 10px;
	}

	.scale {
		position: absolute;
		margin-top: 0;
		margin-left: 5px;
		width: 100%;
	}

	.marker {
		position: absolute;
		margin-top: 2px;
		font-size: 10px;

	}

	.unit-min {
		position: relative;
		float: left;
		margin-left: 5px;
		margin-top: -15px;
		z-index: 3;
		font-size: 12px;
	}

	.unit-max {
		position: relative;
		float: right;
		margin-right: -5px;
		margin-top: -15px;	
		z-index: 3;
		font-size: 12px;
	}

	.vertical-bar {
		transform: rotate(-90deg);
		padding: 10px 0;
		position: relative;
		margin-bottom: 10px;
		width: 45px;
		height: 50px;
	}

	.vertical-scale {
		position: absolute;
		margin-top: 0;
		width: 100%;
	}

	.vertical-marker {
		display: none;
	}

	.vertical-unit-min {
		display: none;
	}

	.vertical-unit-max {
		display: none;
	}
	@import '../assets/fonts/karina-ui-icons/icons.css';

</style>
