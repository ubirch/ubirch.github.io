# Ubirch Trust Service

The ubirch API provides a couple of services that allow the registration of keys, sending and anchoring Ubirch Protocol Packages (UPPs), things administration and the verification of data and seals.

## Service Overview / Short Description
* **Ubirch Niomon Service:** Services for sending and anchoring UPPs into the blockchain.
* **Ubirch Key Service:** Services to register and manage public keys from things.
* **Ubirch Thing API:** Services to register and manage things (devices).
* **Ubirch Verification Service:** Service to verify the authenticity and integrity of data via its UTP package against the blockchain.
* **Ubirch Simple Data Service:** Simple data storage service for demo and test purposes.

<script src="//unpkg.com/swagger-ui-dist@3/swagger-ui-bundle.js"></script>
<script src="//unpkg.com/swagger-ui-dist@3/swagger-ui-standalone-preset.js"></script>
<script type="text/javascript">

  function readTextFile(file, callback) {
    var rawFile = new XMLHttpRequest();
    rawFile.overrideMimeType("application/json");
    rawFile.open("GET", file, true);
    rawFile.onreadystatechange = function() {
      if (rawFile.readyState === 4 && rawFile.status === 200) {
        callback(rawFile.responseText);
      }
    };
    rawFile.send(null);
  }

  //usage:
  readTextFile("./settings/settings.json", function(text){
    var data = JSON.parse(text);
    var swagger_path = data.docu_file_server_uri;
    var paths_file = data.docu_file_server_uri + data.paths_doc;
    console.log(paths_file);
    readTextFile(paths_file, function (text) {
      var data = JSON.parse(text);
      console.log(data);

      var tbl = document.getElementById("docu_table");

      for (var j = 0; j < data.length; j++){

        if (data[j].add_swagger_path == true)
            var base_url = swagger_path;
        else
            var base_url = "";

        for (var i = 0; i < data[j].services.length; i++){
          var row = document.createElement("tr");
          var link = document.createElement("a");
          link.setAttribute('href', '?url=' + base_url + data[j].services[i].uri);
          link.appendChild(document.createTextNode(data[j].services[i].name));
          var cell = document.createElement("td");
          cell.appendChild(link);
          row.appendChild(cell);
          row.appendChild(document.createElement("td"))
            .appendChild(document.createTextNode(data[j].services[i].version));

          tbl.appendChild(row);
        }
      }
      //ToDo: Add further elements to table in a similar pattern!
    });
  });
</script>

## Service List

<table>
  <thead>
  <tr>
    <td><b>Service</b></td>
    <td><b>Version</b></td>
  </tr>
  </thead>
  <tbody id="docu_table">
  </tbody>
</table>

## Swagger Documentation   
<div id="swagger-ui">
</div>

<script>
window.onload = function() {
 var hash = window.location.search.substring(1);
  var regex = /([^&=]+)=([^&]*)/g;
  var m;
  var token = {};

  while (m = regex.exec(hash)) {
    var param = decodeURIComponent(m[1]);
    token[param] = decodeURIComponent(m[2]);
    console.log("token[param]" + token[param]);
  }

  // Build a system
  const ui = SwaggerUIBundle({
      url: token.url ? token.url : "https://niomon.dev.ubirch.com/swagger/swagger.json",
      dom_id: '#swagger-ui',
      deepLinking: true,
      presets: [
          SwaggerUIBundle.presets.apis,
          SwaggerUIStandalonePreset
      ],
      plugins: [
          SwaggerUIBundle.plugins.DownloadUrl
      ],
      layout: "StandaloneLayout",
  })
  window.ui = ui
}
</script>    

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
