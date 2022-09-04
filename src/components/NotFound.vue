<template>
	<div>
		<h1>PAGE NOT FOUND !</h1>
		<img src="./../assets/404.jpg" >
        <a href='https://blog-nvm.herokuapp.com'><button class="btn btn-outline-dark">HOME</button></a>
	</div>
</template>

<script>
export default{
    name : 'NotFound',
	created(){
        document.title = "Blog App - Page Not Found"
    }
}

</script>

<style scoped>
img {
	width: 300px;
	display:block;
	margin-left: 36%;
}
h1{
	margin-top: 30px;
	font-size: 100px;
}
button {
	margin-bottom: 30px;
	display: block;
	margin-left: 44%;
	transition: all 1s ease;
}



/* RESPONSIVE */


@media (min-width : 1243px) and (max-width : 1440px) {

}



/* Responsive Laptop 1024px */

@media (min-width : 1124px) and (max-width : 1243px) {

}
@media (min-width : 1024px) and (max-width : 1124px) {
	h1{
		font-size: 80px;
	}
	button {
		margin-left: 46%;
	}

}

/* Responsive Laptop 1024px */
@media (min-width : 768px) and (max-width : 1024px) {
	h1{
		font-size: 60px;
	}
	button {
		margin-left: 50%;
	}
}

@media (min-width : 425px) and (max-width : 768px) {
	h1{
		font-size: 40px;
	}
	button {
		margin-left: 46%;
	}
	img {
		width:200px;
		margin-left: 30%;
	}
}

/* Responsive Laptop 1024px */
@media (min-width : 375px) and (max-width : 425px) {
	h1{
		font-size: 30px;
	}
	button {
		margin-left: 42%;
	}
	img {
		width:200px;
		margin-left: 25%;
	}
}

@media (min-width : 320px) and (max-width : 375px) {
	h1{
		font-size: 30px;
	}
	button {
		margin-left: 42%;
	}
	img {
		width:200px;
		margin-left: 25%;
	}
}
@media (max-width : 320px) {
	h1{
		font-size: 30px;
	}
	button {
		margin-left: 42%;
	}
	img {
		width:200px;
		margin-left: 25%;
	}
}


</style>