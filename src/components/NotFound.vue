<template>
	<div>
		<h1>PAGE NOT FOUND !</h1>
		<img width="300" src="./../assets/404.jpg" style="display:block;margin-left: 36%;">
        <a href='https://blog-nvm.herokuapp.com'><button class="btn btn-outline-dark">HOME</button></a>
	</div>
</template>

<script>
export default{
    name : 'NotFound'
}

</script>

<style scoped>
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
</style>