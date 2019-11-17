    <div id="vm1">
        <h1>{{ name }}</h1>
    </div>
    
    <div id="vm2">
        <button @click="showName">Show name</button>
    </div>
    <script>
        var vm1 = new Vue({
            el: '#vm1',
            data: {
                name: 'Vue Instance #1'
            }
        });
    
        var vm2 = new Vue({
            el: '#vm2',
            methods: {
                showName: function () {
                    alert("The name of the other Vue instance is: ");
                }
            }
        });
    
        setTimeout(function () {
            vm1.name = 'Hello from the other side!';
        }, 2000);
    
    </script>
    
    
    <!--===============================-->
    
    
    <div id="appa1">
        <label for="thing">field 3.2.1:</label>
        <input id="thing" type="text"/>
        <p class="formname"></p>
    </div>
    <script>
        $(function () {
            //keypress wouldn't include delete key, keyup does. We also query the div id app and find the other elements so that we can reduce lookups
            $('#appa1').keyup(function (e) {
                var formname = $(this).find('.formname');
                //store in a variable to reduce repetition
                var n_input = $(this).find('#thing').val();
                formname.empty();
                formname.append(n_input);
            });
        });
    
    </script>
    
    <div id="appa2">
        <label for="name">field 2.5.3:</label>
        <input id="name" type="text" v-model="name"/> <!--v-model is doing the magic here-->
        <p>{{ name }}</p>
    </div>
    <script>
        //this is a vue instance
        new Vue({
            //this targets the div id app
            el: '#appa2',
            data: {
                name: '' //this stores data values for ‘name’
            }
        })
    </script>
    
    <hr>
    
    <div id="appb1">
        <button type="button" id="toggle_appb1" aria-expanded="false">
            Toggle Panel
        </button>
        <p class="hello_appb1">hello</p>
    </div>
    <script>
        $(function () {
            $('#toggle_appb1').on('click', function () {
                $('.hello_appb1').hide();
    
                if ($(this).attr('aria-expanded') == "false") {
                    $(this).attr('aria-expanded', true);
                    $('.hello_appb1').hide();
                } else {
                    $(this).attr('aria-expanded', false);
                    $('.hello_appb1').show();
                }
    
                // $(this).attr('aria-expanded', ($(this).attr('aria-expanded') == "false" ? true : false));
    
    
            });
        });
    </script>
    
    
    <!--================-->
    <div id="app2x">
        <p v-show="isNinja">Invisible like a ninja!</p>
        <p v-show="!isNinja">Here I am!</p>
    
        <button v-on:click="isNinja = !isNinja">Toggle Ninja Skills</button>
    </div>
    
    <script>
        new Vue({
            el: '#app2x',
            data: {
                isNinja: true
            }
        });
    </script>
    
    <!--===========================-->
    
    
    <div id="appb2">
        <button @click="show = !show" :aria-expanded="show ? 'true' : 'false'">
            Toggle Panel
        </button>
        <p v-if="show">hello</p>
    </div>
    <script>
        new Vue({
            el: '#appb2',
            data: {
                show: true
            }
        })
    
    </script>
    
    <hr>
    
    <div id="appc1">
        <label for="textareac1">What is your favorite kind of chokolet?</label>
        <textarea id="textareac1"></textarea>
        <br>
        <button id="c1" v-show="chokolet">Let us know!</button>
    </div>
    <script>
        $(function () {
            var button = $('button#c1');
            var textarea = $('#textareac1');
    
            button.hide();
            textarea.keyup(function () {
                if (textarea.val().length > 0) {
                    button.show();
                } else {
                    button.hide();
                }
            })
        });
    
    </script>
    
    
    <div id="appc2">
        <label for="textareac2">What is your favorite kind of chokolet?</label>
        <textarea id="textareac2" v-model="chokolet"></textarea>
        <br>
        <button v-show="chokolet">Let us know!</button>
    </div>
    <script>
        new Vue({
            el: '#appc2',
            data() {
                return {
                    chokolet: ''
                }
            }
        })
    </script>
    
    <hr>
    
    <div id="appd1">
        <form id="d1f" action="/">
            <div>
                <label for="name">Name:</label><br>
                <input id="name" type="text" name="name" required/>
            </div>
            <div>
                <label for="email">Email:</label><br>
                <input id="email" type="email" name="email" required/>
            </div>
            <div>
                <label for="caps">HOW DO I TURN OFF CAPS LOCK:</label><br>
                <textarea id="caps" name="caps" required></textarea>
            </div>
            <button class="submit" id="d1b" type="submit">Submit</button>
            <div>
                <h3>Response from server:</h3>
                <pre class="responsec1"></pre>
            </div>
        </form>
    </div>
    
    <script>
        $(function () {
            var button = $("button#d1b");
            var name = $("input[name=name]");
    
            name.keyup(function () {
                if (name.val().length > 0) {
                    button.addClass('active');
                } else {
                    button.removeClass('active');
                }
            });
    
            $("form#d1f").submit(function (event) {
                event.preventDefault();
    
                //get the form data
                var formData = {
                    name: $("input[name=name]").val(),
                    email: $("input[name=email]").val(),
                    caps: 'jhgjhgj'//$("input[name=caps]").val().length>0?$("input[name=caps]").val():'jhgjhgj'
                };
                console.log(formData);
    
                // process the form
                $.ajax({
                    type: "POST",
                    url: "//jsonplaceholder.typicode.com/posts",
                    data: formData,
                    dataType: "json",
                    encode: true
                }).done(function (data) {
                    console.log(data);
                    $(".responsec1")
                        .empty()
                        .append(JSON.stringify(data, null, 2));
                });
            });
        });
    
    
    </script>
    
    
    <div id="app">
        <form @submit.prevent="submitForm">
            <div>
                <label for="name">Name:</label><br>
                <input id="name" type="text" v-model="name" required/>
            </div>
            <div>
                <label for="email">Email:</label><br>
                <input id="email" type="email" v-model="email" required/>
            </div>
            <div>
                <label for="caps">HOW DO I TURN OFF CAPS LOCK:</label><br>
                <textarea id="caps" v-model="caps" required></textarea>
            </div>
            <button :class="[name ? activeClass : '']" type="submit">Submit</button>
            <div>
                <h3>Response from server:</h3>
                <pre>{{ response }}</pre>
            </div>
        </form>
    </div>
    
    <script>
        new Vue({
            el: '#app',
            data() {
                return {
                    name: '',
                    email: '',
                    caps: '',
                    response: '',
                    activeClass: 'active'
                }
            },
            methods: {
                submitForm() {
                    axios.post('//jsonplaceholder.typicode.com/posts', {
                        name: this.name,
                        email: this.email,
                        caps: this.caps
                    }).then(response => {
                        this.response = JSON.stringify(response, null, 2)
                    })
                }
            }
        })
    
    
    </script>
    
    
    <div id="app">
        <ul class="plans">
            <plan-component name="Basic"></plan-component>
    
            <plan-component name="Recreational"></plan-component>
    
            <plan-component name="Team" most-popular></plan-component>
    
            <plan-component name="Club"></plan-component>
        </ul>
    </div>
    
    <template id="plan-component">
        <li v-bind:class="{ 'most-popular': mostPopular }">
            <p>{{ name }} <small v-if="mostPopular" class="popular-plan-label" style="color: red">Most popular</small></p>
        </li>
    </template>
    
    
    
    <div v-if="Math.random() > 0.5">
        Сейчас меня видно
    </div>
    <div v-else>
        А теперь — нет
    </div>
    
    <!--=====================================-->
    <template>
        <div v-if="available">
            <h1>Items are available</h1>
            <p>More items in the stock</p>
        </div>
        <div v-else>
            <h1>Items are not available</h1>
            <p>Out of stock</p>
        </div>
    </template>
    
    <script>
        export default {
            data: function() {
                return {
                    available: true
                };
            }
        };
    </script>
    
    <!--================================-->
    <template>
        <div>
            <!-- v-if="javascript expression" -->
            <h1 v-if="isActive">Hello Vuejs</h1>
            <button @click="isActive= !isActive">Click</button>
        </div>
    </template>
    
    <script>
    
        export default{
            data:function(){
                return{
                    isActive:false
                }
            }
        }
    </script>
    
    <!--=================================--><hr>
    
    <div id="app">
        <p v-if="itemsInStock > 10">{{ itemsInStock }} in stock.</p>
        <p v-else-if="itemsInStock > 0">Hurry up, there are just a few items left!</p>
        <p v-else>Too bad, we're all out!</p>
    
        <template v-if="itemsInStock > 50">
            <p>Special Offer!</p>
            <p>Looks like we have tons of products in stock... Save 20% if you order now!</p>
        </template>
    </div>
    
    
    
    <!--<template v-if="itemsInStock > 50">-->
        <!--<p>Special Offer!</p>-->
        <!--<p>Looks like we have tons of products in stock... Save 20% if you order now!</p>-->
    <!--</template>-->
    <script>
        new Vue({
            el: '#app',
            data: {
                itemsInStock: 51
            }
        });
    </script>
    
    
    
    <div id="app1">
        <input type="text" v-model="searchQuery">
    
        <p v-if="isSearching">Searching...</p>
        <div v-else>
            <ol>
                <li v-for="result in results">{{ result }}</li>
            </ol>
        </div>
    </div>
    
    <script>
        new Vue({
            el: '#app1',
            data: {
                searchQuery: '',
                results: [],
                isSearching: false
            },
            watch: {
                searchQuery: function(query) {
                    this.isSearching = true;
                    var vm = this;
    
                    setTimeout(function() {
                        vm.results = ['JavaScript', 'PHP', 'MySQL'];
                        vm.isSearching = false;
                    }, 500);
                }
            }
        });
    </script>
    
    <!--========================-->
    
    <div id="app">
        <h1>Movies</h1>
    
        <ul>
            <li v-for="title in movieTitles">{{ title }}</li>
        </ul>
    
        <h1>Employees</h1>
    
        <table border="1">
            <thead>
            <tr>
                <th>Name</th>
                <th>Title</th>
                <th>Company</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="employee in employees">
                <td>{{ employee.name }}</td>
                <td>{{ employee.title }}</td>
                <td>{{ companyName }}</td>
            </tr>
            </tbody>
        </table>
    </div>
    
    
    
    <script>
        new Vue({
            el: '#app',
            data: {
                movieTitles: ['The Matrix', 'The Matrix: Reloaded', 'The Matrix Revolutions'],
                employees: [
                    { name: 'Abby', title: 'Accountant' },
                    { name: 'Andy', title: 'Marketing Manager' },
                    { name: 'Brandon', title: 'Vue.js Expert' }
                ],
                companyName: 'VueX Ltd.'
            }
        });
    </script>
    
    
    <!--========index==================-->
    <div id="app2">
        <h1>Employees</h1>
    
        <table border="1">
            <thead>
            <tr>
                <th>Name</th>
                <th>Title</th>
                <th>Company</th>
                <th>Index</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(employee, index) in employees">
                <td>{{ employee.name }}</td>
                <td>{{ employee.title }}</td>
                <td>{{ companyName }}</td>
                <td>{{ index }}</td>
            </tr>
            </tbody>
        </table>
    </div>
    
    <script>
        new Vue({
            el: '#app2',
            data: {
                employees: [
                    { name: 'Abby', title: 'Accountant' },
                    { name: 'Andy', title: 'Marketing Manager' },
                    { name: 'Brandon', title: 'Vue.js Expert' }
                ],
                companyName: 'VueX Ltd.'
            }
        });
    </script>
    
    <!--========================-->
    <div id="app3">
        <ul>
            <li v-for="(n, index) in 10">{{ n }} * {{ index }} = {{ n * index }}</li>
        </ul>
    </div>
    
    <script>
        new Vue({
            el: '#app3'
        });
    </script>
    
    <!--=========================-->
    <div id="app4">
        <ul>
            <li v-for="n in numbers">{{ n }}</li>
        </ul>
    
        <button v-on:click="changeNumber()">Change Number</button>
    </div>
    
    <script>
        new Vue({
            el: '#app4',
            data: {
                numbers: [1, 2, 3, 4, 5]
            },
            methods: {
                changeNumber: function() {
                    Vue.set(this.numbers, 1, 10);
                    // alert(this.numbers[1]);
                }
            }
        });
    </script>
    
    <!--================================-->
    <div id="app5">
        <ul>
            <li v-for="p in persons" v-bind:key="p.id">{{ p.name }} (ID: {{ p.id }}) <input type="text"></li>
        </ul>
    
        <button v-on:click="addNewPerson()">Add Person</button>
        <button v-on:click="shuffle()">Shuffle</button>
    </div>
    
    <script>
        new Vue({
            el: '#app5',
            data: {
                persons: [
                    { id: 1, name: 'Andy' },
                    { id: 2, name: 'Abby' },
                    { id: 3, name: 'Teresa' }
                ]
            },
            methods: {
                addNewPerson: function() {
                    var highestId = Math.max.apply(Math, this.persons.map(function(p) {
                        return p.id;
                    }));
                    var names = ['Billy', 'Michael', 'Steve', 'Diane', 'Peter'];
    
                    this.persons.push({
                        id: highestId + 1,
                        name: names[Math.floor(Math.random() * names.length)]
                    });
                },
                shuffle: function() {
                    this.persons = this.shuffleArray(this.persons);
                },
                shuffleArray: function (arr) {
                    var newArr = arr.slice();
    
                    for (var i = newArr.length - 1; i > 0; i--) {
                        var j = Math.floor(Math.random() * (i + 1));
                        var temp = newArr[i];
                        newArr[i] = newArr[j];
                        newArr[j] = temp;
                    }
    
                    return newArr;
                }
            }
        });
    </script>
    
    
    
    
