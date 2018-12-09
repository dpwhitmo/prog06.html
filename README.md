
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Prog 4</title>
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
        .vert-divider 
        {
            margin-left: 5px;
            margin-right: 5px;
            width: 1px;
            height: 120px;
            display: inline-block;
        }
        #users-form{
            max-height: 300px;
            overflow-y: auto;
            overflow-x: hidden;
        }
    </style>
</head>

<body onload="addForm()">
    <div class="container">
        <h1>Add new input lines to HTML entry form</h1>
        <p>Enable users to add a row of fields to an HTML input form. In this
            code, after the HTML loads, addForm() is called. Clicking the Add Line
            button calls a function, addLine(), which calls addField() once for each
            field in the new input line. The elements, label and input, are
            formatted using CSS. </p>


        <button class="btn btn-primary" onclick="addRow()">Add Line</button>
        <div id="form-container" class="mb-4"></div>
        
        <h2>Generate array of objects</h2>
        <p>Loop through HTML form lines and store the data into an array of objects</p>
        <button class="btn btn-secondary" onclick="genObjectArray()">Generate and Display Array of Objects</button>
        <div id="object-display" class="mb-4"></div>

        <h2>Generate a table containing data from array of objects</h2>
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
    </div>

    <script>
        var rowCount = 0
        var objects = []

        var zodiacUSA = 
        [
            "Capricorn", "Aquarius", "Pisces", "Aries", "Taurus", "Gemini",
            "Cancer", "Leo", "Virgo", "Libra", "Scorpio", "Sagittarius"
        ]
        var zodiacChina = 
        [
            "Monkey", "Rooster", "Dog", "Pig", "Rat", "Ox",
            "Tiger", "Rabbit", "Dragon", "Snake", "Horse", "Goat"
        ]


        function addForm() 
        {
            var form = $("#form-container").append($("<form></form>").attr("id", "users-form"))
            addRow()
        }



        function addRow() 
        {

            var form = $("#users-form")

            /* Modularize field creation since it's all basically the same */
            function genField(label, name, type, placeholder) 
            {
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

            function genSelect(label, name, items) 
            {
                var cell = $("<div></div>").addClass("col")
                $("<label></label>")
                    .text(label)
                    .attr("for", name)
                    .appendTo(cell)
                let select = $("<select></select>")
                    .addClass("form-control")
                    .attr("id", name)
                    .appendTo(cell)

                for (var i = 0; i < items.length; i += 1) 
                {
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

            row.append(genSelect("Country", "country" + rowCount, 
            [
                { value: "USA", text: "USA" },
                { value: "China", text: "China" }
            ]))
            form.append(row)
            rowCount += 1
        }

        function genObjectArray() 
        {
            // Reset the objects array
            objects = []

            for (var i = 0; i < rowCount; i += 1) 
            {

                // Get values
                let fName = $("#fname" + i).val()
                let sName = $("#sname" + i).val()
                let birthday = $("#birthday" + i).val()
                let birthmonth = $("#birthmonth" + i).val()
                let birthyear = $("#birthyear" + i).val()
                let country = $("#country" + i).val()

                // Push into array
                let obj = 
                {
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

        function displayObjects() 
        {
            let text = ""
            for (var i = 0; i < objects.length; i += 1) 
            {
                let obj = objects[i]
                let output = "" + i + ". " + JSON.stringify(obj) + " .. calculateZodiac() == "
                    + calculateZodiac(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)
                    + " .. localizeDate() == " + localizeDate(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)
                    + " .. solarAge() == " + solarAge(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country) + "<br/>"
                text += output
            }
            return text
        }

        function calculateZodiac(month, day, year, country) 
        {
            month = parseInt(month)
            day = parseInt(day)
            year = parseInt(year)
            if (country == "USA") {
                switch (month) {
                    case 1:
                        if (day < 20)
                            return zodiacUSA[0]
                        return zodiacUSA[1]
                    case 2:
                        if (day < 19)
                            return zodiacUSA[1]
                        return zodiacUSA[2]
                    case 3:
                        if (day < 21)
                            return zodiacUSA[2]
                        return zodiacUSA[3]
                    case 4:
                        if (day < 20)
                            return zodiacUSA[3]
                        return zodiacUSA[4]
                    case 5:
                        if (day < 20)
                            return zodiacUSA[4]
                        return zodiacUSA[5]
                    case 6:
                        if (day < 20)
                            return zodiacUSA[5]
                        return zodiacUSA[6]
                    case 7:
                        if (day < 20)
                            return zodiacUSA[6]
                        return zodiacUSA[7]
                    case 8:
                        if (day < 20)
                            return zodiacUSA[7]
                        return zodiacUSA[8]
                    case 9:
                        if (day < 20)
                            return zodiacUSA[8]
                        return zodiacUSA[9]
                    case 10:
                        if (day < 20)
                            return zodiacUSA[9]
                        return zodiacUSA[10]
                    case 11:
                        if (day < 20)
                            return zodiacUSA[10]
                        return zodiacUSA[11]
                    case 12:
                        if (day < 20)
                            return zodiacUSA[11]
                        return zodiacUSA[0]
                }
            } else {
                return zodiacChina[year % 12]
            }
        }

        function solarAge(month, day, year, country) 
        {
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

        function localizeDate(month, day, year, country) 
        {
            month = ("0" + month).slice(-2)
            day = ("0" + day).slice(-2)
            year = ("000" + year).slice(-4)
            if (country == "USA") {
                return month + "-" + day + "-" + year
            } else {
                return year + "-" + month + "-" + day
            }
        }

        function generateTable() 
        {

            function createRow(obj) 
            {
                let row = $("<tr></tr>")

                $("<td></td>").text(obj.name + " " + obj.surname).appendTo(row)
                $("<td></td>").text(localizeDate(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)).appendTo(row)
                $("<td></td>").text(solarAge(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)).appendTo(row)
                $("<td></td>").text(calculateZodiac(obj.birthMonth, obj.birthDay, obj.birthYear, obj.country)).appendTo(row)
                return row
            }

            let tbody = $("#output-table").html("")

            $.each(objects, function(key, value)
            {
                createRow(value).appendTo(tbody)
            })
        }
    </script>
</body>

</html>
