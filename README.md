<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Prog 6</title>
      <script
         src="https://code.jquery.com/jquery-3.3.1.min.js"
         integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
         crossorigin="anonymous"></script>
      <!-- Include bootstrap 4 for styling -->
      <!--<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>-->
      <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO"
         crossorigin="anonymous">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <style>
         .vert-divider {
         margin-left: 5px;
         margin-right: 5px;
         width: 1px;
         height: 100px;
         display: inline-block;
         background: #dedede;
         }
         #users-form{
         max-height: 400px;
         overflow-y: auto;
         overflow-x: hidden;
         }
      </style>
   </head>
   <body onload="addForm()">
      <div class="container">
         <h1>Add new input lines to HTML entry form</h1>
         <p>Please enter the number of people you would like entered. For more people, click the "Add Line" button. </p>
         <button class="btn btn-primary" onclick="addRow()">Add Line</button>
         <div id="form-container" class="mb-4"></div>
         <h2>Generate array of objects</h2>
         <p>Loop through HTML form lines and store the data into an array of objects</p>
         <button class="btn btn-secondary" onclick="genObjectArray()">Generate and Display Array of Objects</button>
         <div id="object-display" class="mb-4"></div>
         <h2>Generate a table from the array of objects</h2>
         <button class="btn btn-primary" onclick="generateTable()">Generate and Display Table</button>
         <table class="table table-striped">
            <thead>
               <tr>
                  <th>Local Name</th>
                  <th>Local DOB</th>
                  <th>Local Age</th>
                  <th>Local Zodiac</th>
               </tr>
            </thead>
            <tbody id="output-table">
            </tbody>
         </table>
         <br></br>
         <br></br>
         <br></br>
         <p> Differences Between Jquery and Javascript </p>
         <ol>
            <li> Special Symbol "$" is used in place of the word jQuery and this exist in jQuery only. Javascript: var container =                      document.getElementById('container'); JQuery: var container = $("#container"); </li>
            <li> jQuery uses a library url and that must be incuded in the head of the html doucument</li> 
            <li> jQuery features an .addClass method for adding a class. Plain Javascript uses .classList.add</li>
            <li> Unlike Javascript, many JQuery methods return a JQuery object, not a raw DOM object.</li>
            <li> jQuery allows callback functions once execution has occured</li>
            
         </ol>
      </div>
      <script>
         var rowCount = 0
         var objects = []
         
         function addForm() {
             var form = $("#form-container").append($("<form></form>").attr("id", "users-form"))
             addRow()
         }
         
         
         
         function addRow() {
         
             var form = $("#users-form")
         
             /* Modularize field creation since it's all basically the same */
             function genField(label, name, type, placeholder) {
                 var cell = $("<div></div>").addClass("col")
                 $("<label></label>").text(label).attr("for", name).appendTo(cell)
                 $("<input>")
                     .addClass("form-control")
                     .attr({
                         type: "text",
                         id: name,
                         name: name,
                         placeholder: placeholder
                     })
                     .appendTo(cell)
                 return cell
             }
         
             function genSelect(label, name, items) {
                 var cell = $("<div></div>").addClass("col")
                 $("<label></label>")
                     .text(label)
                     .attr("for", name)
                     .appendTo(cell)
                 let select = $("<select></select>")
                     .addClass("form-control")
                     .attr("id", name)
                     .appendTo(cell)
         
                 for (var i = 0; i < items.length; i += 1) {
                     $("<option></option>")
                         .text(items[i].text)
                         .attr("value", items[i].value)
                         .appendTo(select)
                 }
                 return cell
             }
         
             var row = $("<div></div>")
                 .addClass("form-row")
                 .attr("id", "form-row-" + rowCount)
         
             row.append(genField("Given Name", "fname" + rowCount, "text", ""))
             row.append(genField("Surname", "sname" + rowCount, "text", ""))
         
             $("<div></div>").addClass("vert-divider").appendTo(row)
         
             row.append(genField("Solar Birth Day", "birthday" + rowCount, "number", ""))
             row.append(genField("Solar Birth Month", "birthmonth" + rowCount, "number", ""))
             row.append(genField("Solar Birth Year", "birthyear" + rowCount, "number", ""))
         
             row.append(genSelect("Country", "country" + rowCount, [
                 { value: "USA", text: "USA" },
                 { value: "China", text: "China" }
             ]))
             form.append(row)
             rowCount += 1
         }
         
         function genObjectArray() {
             // Reset the objects array
             objects = []
         
             for (var i = 0; i < rowCount; i += 1) {
         
                 // Get values
                 let fName = $("#fname" + i).val()
                 let sName = $("#sname" + i).val()
                 let birthday = $("#birthday" + i).val()
                 let birthmonth = $("#birthmonth" + i).val()
                 let birthyear = $("#birthyear" + i).val()
                 let country = $("#country" + i).val()
         
                 // Push into array
                 let obj = {
                     name: fName,
                     surname: sName,
                     birthDay: birthday,
                     birthMonth: birthmonth,
                     birthYear: birthyear,
                     country: country
                 }
                 objects.push(obj)
             }
         
             // Output
             let display = $("#object-display").html(displayObjects())
             console.log(objects)
         
         }
         
         function displayObjects() {
             let text = ""
             for (var i = 0; i < objects.length; i += 1) {
                 let obj = objects[i]
                 let output = "" + i + ". " + JSON.stringify(obj) + " .. calculateZodiac() == "
                     + calculateZodiac(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)
                     + " .. localizeDate() == " + localizeDate(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)
                     + " .. solarAge() == " + solarAge(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country) + "<br/>"
                 text += output
             }
             return text
         }
         
         function calculateZodiac(month, day, year, country) {
             month = parseInt(month)
             day = parseInt(day)
             year = parseInt(year)
         if(country  == "USA") {
         if ((month == 04 && 20 <=day) || (month == 05 && 20 >= day)) {
         return "Taurus";
         }
         else if ((month == 05 && 21 <=day) || (month == 06 && 20 >= day)){
         return "Gemini";
         }
         else if ((month == 06 && 21 <=day) || (month == 07 && 22 >= day)){
         return "Cancer";
         }
         else if ((month == 07 && 23 <=day) || (month == 08 && 22 >= day)){
         return "Leo";
         }
         else if ((month == 08 && 23 <=day) || (month == 09 && 22 >= day)){
         return "Virgo";
         }
         else if ((month == 09 && 23 <=day) || (month == 10 && 22 >= day)){
         return "Libra";
         }
         else if ((month == 10 && 23 <=day) || (month == 11 && 21 >= day)){
         return "Scorpio";
         }
         else if ((month == 11 && 22 <=day) || (month == 12 && 21 >= day)){
         return "Sagittarius";
         }
         else if ((month == 12 && 22 <=day) || (month == 01 && 19 >= day)){
         return "Capricorn";
         }
         else if ((month == 01 && 20 <=day) || (month == 02 && 18 >= day)){
         return "Aquarius";
         }
         else if ((month == 03 && 21 <= day) || (month == 04 && 19 >= day)) {
         return "Aries";
         }
         else {
         return "Pisces";
         }
         }
         
         // Chinese zodiac
         else if (country == "China") {
         if (year % 12 == 0) { 
         return "Monkey";
         }
         else if (year % 12 == 1) { 
         return "Rooster";
         }
         else if (year % 12 == 2) { 
         return "Dog";
         }
         else if (year % 12 == 3) { 
         return "Pig";
         }
         else if (year % 12 == 4) {
         return "Rat";
         }
         else if (year % 12 == 5) {
         return "Ox";
         }
         else if (year % 12 == 6) {
         return "Tiger";
         }
         else if (year % 12 == 7) {
         return "Rabbit";
         }
         else if (year % 12 == 8) {
         return "Dragon";
         }
         else if (year % 12 == 9) {
         return "Snake";
         }
         else if (year % 12 == 10) {
         return "Horse";
         }
         else {
         return "Sheep";
         }
         }
         }
         
         function solarAge(month, day, year, country) {
             month = parseInt(month)
             day = parseInt(day)
             year = parseInt(year)
             let now = Date.now()
             let birthday = new Date(year, month - 1, day)
             let difference = new Date(now - birthday)
         
             console.log(difference.getYear() - 70)
             if (country == "USA") {
         
                 return difference.getYear() - 70
             }
             return difference.getYear() - 70 + 1
         }
         
         function localizeDate(month, day, year, country) {
             month = ("0" + month).slice(-2)
             day = ("0" + day).slice(-2)
             year = ("000" + year).slice(-4)
             if (country == "USA") {
                 return month + "-" + day + "-" + year
             } else {
                 return year + "-" + month + "-" + day
             }
         }
         
         function generateTable() {
         
             function createRow(obj) {
                 let row = $("<tr></tr>")
         
                 $("<td></td>").text(obj.name + " " + obj.surname).appendTo(row)
                 $("<td></td>").text(localizeDate(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)).appendTo(row)
                 $("<td></td>").text(solarAge(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)).appendTo(row)
                 $("<td></td>").text(calculateZodiac(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)).appendTo(row)
                 return row
             }
         
             let tbody = $("#output-table").html("")
         
             $.each(objects, function(key, value){
                 createRow(value).appendTo(tbody)
             })
         }
      </script>
   </body>
</html>
