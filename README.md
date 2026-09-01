# Consulta NF-e
App auxiliar para consultar a chave de acesso da NFe nos ambientes de Produção ou Homologação


Para preencher automaticamente a chave no site da NF-e, instale o [Tampermonkey](https://www.tampermonkey.net/ "Tampermonkey") 

E importe o seguinte script:

```javascript
// ==UserScript==
// @name         Preencher Chave NF-e SEFAZ
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Preenche automaticamente o campo da chave na página de consulta da SEFAZ
// @author       Você
// @match        https://hom.nfe.fazenda.gov.br/portal/consultaRecaptcha.aspx*
// @match        https://www.nfe.fazenda.gov.br/portal/consultaRecaptcha.aspx*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Aguarda o carregamento completo da página
    window.addEventListener('load', function() {
        // Obtém a chave da URL (parâmetro "ChaveAcesso")
        const urlParams = new URLSearchParams(window.location.search);
        const chave = urlParams.get('ChaveAcesso');

        if (!chave) {
            console.log('[Userscript] Nenhuma chave encontrada na URL.');
            return;
        }

        // Localiza o campo de entrada
        const campo = document.getElementById('ctl00_ContentPlaceHolder1_txtChaveAcessoResumo');
        if (campo) {
            campo.value = chave;
            console.log('[Userscript] Chave preenchida com sucesso:', chave);
            // Opcional: disparar eventos para garantir que a validação seja ativada
            campo.dispatchEvent(new Event('input', { bubbles: true }));
            campo.dispatchEvent(new Event('change', { bubbles: true }));
        } else {
            console.log('[Userscript] Campo não encontrado.');
        }
    });
})();
```
