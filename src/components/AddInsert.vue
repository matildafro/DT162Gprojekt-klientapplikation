<template>
    <div>
    <!--förhindrar att sidan läses om-->
    <form @submit.prevent="addInsert()">
        <h2>Börja med att lägga till dina lästa kurser</h2>
        <p id="message"></p>
        <label for="name">Kursnamn:</label>
        <input v-model="name" type="text" id="name" name="name"><br>

        <label for="uni">Universitet:</label>
        <input v-model="uni" type="text" id="uni" name="uni"><br>

        <label for="orient">Inriktning:</label>
        <input v-model="orient" type="text" id="orient" name="orient"><br>

        <label for="points">Poäng:</label>
        <input v-model="points" type="text" id="points" name="points" placeholder="Halva poäng bryts med punkt"> p<br>

        <input type="submit" value="Lägg till">
    </form>
  
</div>

</template>

<script>
    export default {
        data() {
            //returnerar input-fält som tomma strängar 
            return {
                name: "",
                uni: "",
                orient: "",
                points: ""
            }
        },
        emits: ["insertAdded"],
        methods: {
            async addInsert() {
               //Kontrollerar att fälten är ifyllda
               if(this.name != 0 & this.uni != 0 & this.orient != 0 & this.points != 0) { 
                    //Skapar objekt av det som står i fälten med hjälp av this
                    let InsertBody = {
                        name: this.name,
                        uni: this.uni,
                        orient: this.orient,
                        points: this.points
                    };
 
                    //FETCH
                    const resp = await fetch ("http://localhost:3000/inserts/", {
                        method: "POST",
                        headers: {
                            "Accept": "application/json",
                            "Content-type": "application/json",
                        },
                        body: JSON.stringify(InsertBody)
                    });
                    const data = await resp.json();
                    //tömmer input-fält efter data har skickats
                    this.name = "";
                    this.uni = "";
                    this.orient = "";
                    this.points = ""; 

                   
                    //emit för att ladda om sidan
                    this.$emit("insertAdded");
                    //läser ut meddelande att kurs är tillagd
                    document.getElementById("message").innerHTML = "<p>" + "Kurs tillagd till listan!" + "</p>";

                }else { //felmeddelande om fält är tomma
                    document.getElementById("message").innerHTML = "<p>" + "Vänligen fyll i all information om kursen!" + "</p>";
                }
            },
        }
    }
</script>

<style scoped>
div {
    background-color: white;
    width: 70%;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 10px;
    border-radius: 3px;
    border: 2px solid rgb(105, 180, 180);
    padding: 20px;
}

form {
    text-align: center;
}


input {
    width: 30%;
    margin: 5px;
}

input[type=submit] {
    background-color: darkslategray;
    color: white;
    cursor: pointer;
    padding: 5px;
}
</style>