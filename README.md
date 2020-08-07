# Detectar usuários do AdBlock com o Google Analytics
### Javascript

Um **script** útil para saber quantos usuários, que visitam seu site, usam e ativaram o famoso plug-in / extensão do **AdBlock** para bloquear o conteúdo da publicidade. E para obter esses dados, contaremos com um serviço amplamente usado, o **Google Analytics**.

Primeiro, vamos criar um novo arquivo dentro da **pasta do tema**, que chamaremos de ads.js e inseri-lo no caminho /js/ads.js da pasta do seu tema do WordPress.

Feito isso, insira o código a seguir no arquivo ads.js , que criará uma div muito simples com o ID "**miniadblock**".

```JavaScript
var e=document.createElement('div');
e.id='miniadblock';
e.style.display='none';
document.body.appendChild(e);
```
Agora basta incluir o arquivo JS que acabou de criar, inserindo a função **wp_enqueue_script** no seu arquivo functions.php

```php
wp_enqueue_script( 'mini-adblock', get_template_directory_uri() . '/js/ads.js', array('jquery'), false, true);
```
Depois de incluir o arquivo, você pode colocar, por exemplo, no footer.php do seu tema, o código a seguir, que a cada visita de um usuário que tenha **o Adblock ativo**, criará um novo evento no **Google Analytics**. Obviamente, esse código deve ser inserido após todos os uploads dos arquivos javascript do seu tema, precisamente no arquivo footer.php, geralmente **após** a função wp_footer() e **após o código de rastreamento do Google Analytics**.

```html
<script>
if ( !document.getElementById('miniadblock') ){
	if(typeof ga !=='undefined'){
		ga('send','event','AdBlock User','Yes',{'nonInteraction':1});
	} else if(typeof _gaq !=='undefined'){
		_gaq.push(['_trackEvent','AdBlock User','Yes',undefined,undefined,true]);
	}
}
</script>
```
Feito! Agora você saberá em tempo real quantas pessoas usam o AdBlock no seu site.
