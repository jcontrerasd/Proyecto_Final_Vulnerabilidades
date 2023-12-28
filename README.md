# Ethereum: Seguridad y Buenas Prácticas (DApps)
## -Sprint 4- ## Proyecto Final de Auditoria



<img width="650" alt="image" src="https://github.com/jcontrerasd/Proyecto_Final_Vulnerabilidades/assets/27821228/124524a6-9f13-4f39-bb15-9c9ee5a7ea82">


  _Jaime Contreras | Master en Blockchain, Metaverso y NFT's | Diciembre 2023_

* ### [Informe de Auditoria](https://github.com/jcontrerasd/Proyecto_Final_Vulnerabilidades/blob/main/audit/Sprint%204%20-%20Programaci%C3%B3n%20segura%20en%20Solidity.pdf)


 
<img width="720" alt="image" src="https://github.com/jcontrerasd/Proyecto_Final_Vulnerabilidades/assets/27821228/665df056-c431-4400-86bb-c92ada16d416">

  
<details>
<summary>slither ⚙️</summary>

```js
slither . --checklist --show-ignored-findings --markdown-root ../ >
./audit/iebs_analisis_vulnerabilidades.md

var MemoriaUrbanaToken = artifacts.require("./MemoriaUrbanaToken.sol");
var Market_Place = artifacts.require("./Market_Place.sol");

// El deploy debe ser anidado, dado que el contrato Marketplace requiere el contrato con el que
// estará vinculado

module.exports = function (deployer) {

  deployer.deploy(MemoriaUrbanaToken).then(function () {      
      return deployer.deploy(Market_Place, MemoriaUrbanaToken.address);
  });
};
```
</details>
