# Simple modal system back-end
``` 
const modalSystem = {
	defaultExpireTime: 7, // Em quantos dias irá expirar o cookie
	modalTarget: document.querySelector("#myModal"), //id do modal
	modalButton: document.querySelector(".btn-hide"), //id ou classe do botão de fechar
	getCookie: function(cname){
		var name = cname + "=";
		var ca = document.cookie.split(';');
		for(var i = 0; i < ca.length; i++) {
			var c = ca[i];
			while (c.charAt(0) === ' ') {
			  c = c.substring(1);
			}
			
			if (c.indexOf(name) === 0) {
			  return c.substring(name.length, c.length);
			}
		}
		return null;
	},
	
	setCookie: function(cname, cvalue, exdays) {
	  var d = new Date();
	  d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
	  var expires = "expires="+d.toUTCString();
	  document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
	},
	
	showModal: function(){
		//Função para abrir o modal - Normalmente o modal abre após 5s
		setTimeout(function() {
			if(modalTarget !== null){
				//$(modalTarget).modal("show"); //No caso de bootstrap
				console.log("mostrando modal...")
			}
		}, 5000);
	},
	
	checkClick: function(){
		if(modalButton !== null){
			//Se clicar no botão do modal
			modalButton.addEventListener("click", function(){
				modalSystem.setCookie("openModal", false, modalSystem.defaultExpireTime)	
			});
		}
	},
	
	init: function(){
		if(this.getCookie("openModal") === null){
			//Se não existir o modal, ele cria o cooki e abre o modal
			this.setCookie("openModal", true, this.defaultExpireTime);
			this.showModal();
			
		}else{
			//Se existir o cookie e o cliente não clicou no botão de fechar ele só abre o modal
			if(this.getCookie("openModal") === true){
				this.showModal();
			}
		}
	}
}

window.addEventListener("load", function(){
	modalSystem.init()
})

```
