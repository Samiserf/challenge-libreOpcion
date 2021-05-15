<template>
	<div class="container-options">
		<div class="title-option">
			<div class="title-content">
				<h5>{{value.descripcion}}</h5>
				<i v-if=" value.descripcion.includes('sucursal.')" class="fas fa-building"></i>
				<i v-if=" value.descripcion.includes('Moto')" class="fas fa-motorcycle"></i>
				<i v-if=" value.descripcion.includes('domicilio.')" class="fas fa-truck"></i>
			</div>
		</div>
		<div class="desc-option-total">
			<h6><strong>Total: </strong><span class="total-free" v-if=" value.Total == 0">Gratis</span><span v-else>${{value.Total}}</span></h6>
		</div>
		<div class="desc-option-plazo">
			<span class="disclaimer"><strong>LLega: </strong>{{value.PlazoEntrega}}</span>
		</div>
	</div>
</template>

<script>

export default {
	
	props: {
		value: {
			type: Object,
			required: true
		}
	}

}
</script>

<style lang="scss" scoped>
	.container-options{
		display:flex;
		flex-direction: column;
		width: 45%;
		justify-content: space-between;
		align-items:flex-start;
		margin:20px 20px 0 0;
		font-size:15px;
		padding:15px;
		text-align: left;
		background: #f1f1f1;
		height: 180px;
		&:hover{
			background: #faf8f8;
			transform: scale(1.04);
			transition: all ease 1s;
			cursor: pointer;
			-webkit-box-shadow: 21px 22px 78px -60px rgba(0,0,0,0.75);
			-moz-box-shadow: 21px 22px 78px -60px rgba(0,0,0,0.75);
			box-shadow: 21px 22px 78px -60px rgba(0,0,0,0.75);
		}
		@media (min-width:767px) and (max-width: 992px){
			width: 80%;
		}
		@media (max-width: 766px){
			width: 100%;
		}
		.title-option{
			display:inline-block;
			// text-align: center;
			width: 100%;
			.title-content{
				display: flex;
				flex-direction: row;
				align-items: center;
				
				h5{
					color: #212529;
					padding-right: 10px;
					
				}
			}
		}
		.desc-option-total{
			width: 100%;
			color: #373e44;

			.total-free{
				color:green;
			}
		}
		.desc-option-plazo{
			.disclaimer{
				font-size: 12px;
				color: #9c9b9b
			}
		}
					
	}

</style>
