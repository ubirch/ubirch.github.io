# REST API Documentation

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
        var row = document.createElement("tr");
        row.setAttribute('class', 'info');
        var header = document.createElement("td");
        header.setAttribute('colspan', '3');
        header.appendChild(document.createTextNode(data[j].bundlename));
        row.appendChild(header);
        tbl.appendChild(row);

        for (var i = 0; i < data[j].services.length; i++){
          var row = document.createElement("tr");
          row.appendChild(document.createElement("td"));
          var link = document.createElement("a");
          link.setAttribute('href', '?url=' + swagger_path + data[j].services[i].uri);
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

## Services

<table>
  <thead>
  <tr>
    <td><b>Service</b></td>
    <td></td>
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
    console.log("onload");
 var hash = window.location.search.substring(1);
  var regex = /([^&=]+)=([^&]*)/g;
  var m;
  var token = {};

    console.log("before while");
  while (m = regex.exec(hash)) {
    var param = decodeURIComponent(m[1]);
    token[param] = decodeURIComponent(m[2]);
    console.log("token[param]" + token[param]);
  }
    console.log("after while");

  // Build a system
  const ui = SwaggerUIBundle({
      url: token.url ? token.url : "https://raw.githubusercontent.com/ubirch/ubirchApiDocs/master/swaggerDocs/ubirch/avatar_service/1.0/ubirch_avatar_service_api.json",
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
