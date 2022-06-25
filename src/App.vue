<template>
    <div class="body" style="margin:0; width:100%; min-width:320px">
        <div class="header">
            <h3 style="height:100%; margin:0; padding-left:10px;">Nutrient & Calorie Checker ✓</h3>
            <button class="menuBtn" @click="isOpen = !isOpen" :class="{'open' : isOpen === true}" style="top:0%; height:30px; width:40px; margin:0 5px 0 auto; padding:0; border-radius:50px; background-color:#fff; z-index:500;">
                <span style="top:-8%; right:-25%"></span>
                <span style="top:18%; right:25%"></span>
                <span style="top:-8%; right:0"></span>
            </button>
            <transition name="fade" mode="out-in" appear>
                <div v-if="isOpen === true" style="position:fixed; top:0; left:0; z-index:100; width:100%; height:120%; background-color: rgba(0, 0, 0, 0.6)">
                    <ul  class="menuWindow" style="list-style:none; width:130px; position:absolute; top:40px; right:0%; padding:0; margin:0; border:0.5px solid #dedede; background-color:#333333; z-index:1;">
                        <li>Profile</li>
                        <li>Login</li>
                        <li>Sign Up</li>
                        <li>Contact</li>
                    </ul>
                </div>
            </transition>
        </div>
        <div class="container">
            <ul class="nutrientBoards" style="position:absolute; width:100%; height:225px; top:90px; margin:0 auto; padding:0;" >
                <div class="board boardLeft" style="height:100%; width:35%; padding-right:13%; padding-left:2%">
                    <li class="nutrientBox boxLeft" style="%;">
                        <p>Protein</p>
                        <p class="amount">{{ totalProtein }}g</p>
                    </li>
                    <li class="nutrientBox boxLeft" style="">
                        <p>Carbs</p>
                        <p class="amount">{{ totalCarbs }}g</p>
                    </li>
                    <li class="nutrientBox boxLeft" style="">
                        <p>Sodium</p>
                        <p class="amount">{{ totalSodium }}mg</p>
                    </li>
                </div>
                <div class="board boardRight" style="position:relative; right:0%; height:100%; width:35%; padding-left:13%; padding-right:2%;">
                    <li class="nutrientBox boxRight" style="">
                        <p>Fats</p>
                        <p class="amount">{{ totalFats }}g</p>
                    </li>
                    <li class="nutrientBox boxRight" style="">
                        <p>Vitamin C</p>
                        <p class="amount">{{ totalVitamin }}% <span style="font-size:10px">(DV)</span></p>
                    </li>
                </div>
            </ul>
            <div class="calorieCheckForm">
                <h2 style="margin:auto 0">Target Calorie Intake</h2>
                <input class="inputNumber" type="number" v-model="targetCalories">
                <div style="position:relative; width:200px; height:200px; margin: auto; transform: scale(0.9)">
                    <div class="backgroundCircle" style="position:absolute; width:200px; height:200px; background-color:#dedede; border:10px solid #fff; box-sizing:border-box; border-radius:100px;"></div>
                    <!-- right side of the circle -->
                    <div class="square" style="right:0">
                        <div class="square" style="right:100%; transform-origin: 100% 50%;" :style="rightAngle">
                            <div :class="{'circleRed': totalCalories >= targetCalories }" class="circle" style="left:0;"></div>
                        </div>
                    </div>
                    <!-- left side of the circle-->
                    <div class="square" style="left:0;">
                        <div class="square" style="left:100%; transform-origin: 0% 50%;" :style="leftAngle">
                            <div :class="{'circleRed': totalCalories >= targetCalories }" class="circle" style="right:0;"></div>
                        </div>
                    </div>
                    <h3 style="position:absolute; margin: 0 auto; left:20%; top:45%;">
                    <span :class="{'gray': totalCalories < targetCalories }">
                        <span v-if="totalCalories < 10">000</span>
                        <span v-else-if="totalCalories < 100">00</span>
                        <span v-else-if="totalCalories < 1000">0</span>
                        <span></span>{{ Math.round(totalCalories) }} / 
                    </span>{{ targetCalories }}
                    <br><h4 style="font-size:18px; margin:0%; color:gray">kcal</h4>
                    </h3>
                    
                </div>
                <h3  style="position:absolute; margin: 0 auto; left:33%%; top:63%;" v-if="targetCalories === ''">0</h3>
            </div>
            <div class="selectedItemArea" style="width:95%; margin:0 auto;">
                <table border="2" bordercolor="#fff" bgcolor="#333333" style="width:100%; margin:0 auto; color:#fff; border-radius:5px; box-shadow:0 3px #333333">
                    <tr>
                        <th>Meat</th>
                        <th>Vegetable</th>
                        <th>Fruit</th>
                        <th>Dairy</th>
                    </tr>
                    <tr>
                        <td>
                            <div v-for="meat in meats" :key="meat">
                                <li class="itemBlock" v-if="meat.count > 0">
                                    <img :src="meat.icon" alt="">
                                    <h5>{{ meat.count }}g</h5>
                                </li>
                            </div>
                        </td>
                        <td>
                            <div v-for="vegetable in vegetables" :key="vegetable">
                                <li class="itemBlock" v-if="vegetable.count > 0">
                                    <img :src="vegetable.icon" alt="">
                                    <h5>{{ vegetable.count }}g</h5>
                                </li>
                            </div>
                        </td>
                        <td>
                            <div v-for="fruit in fruits" :key="fruit">
                                <li class="itemBlock" v-if="fruit.count > 0">
                                    <img :src="fruit.icon" alt="">
                                    <h5>{{ fruit.count }}g</h5>
                                </li>
                            </div>
                        </td>
                        <td>
                            <div v-for="dairy in dairies" :key="dairy">
                                <li class="itemBlock" v-if="dairy.count > 0">
                                    <img :src="dairy.icon" alt="">
                                    <h5>{{ dairy.count }}g</h5>
                                </li>
                            </div>
                        </td>
                    </tr>
                    </table>
            </div>
            <div class="foodDisplay">
                <ul class="foodTab">
                    <li  class="tabButton" id="meat" @click="changeTab('1')" :class="{'active': tabNumber === '1'}">Meat</li>
                    <li  class="tabButton" id="vegetable" @click="changeTab('2')" :class="{'active': tabNumber ==='2'}">Vegetable</li>
                    <li  class="tabButton" id="fruit" @click="changeTab('3')" :class="{'active': tabNumber === '3'}">Fruit</li>
                    <li class="tabButton" id="dairy" @click="changeTab('4')" :class="{'active': tabNumber === '4'}">Dairy</li>
                </ul>
                <ul class="foodList" style=" width:100%; margin:0 auto; padding:0;">
                    <li class="meatList" v-if="tabNumber === '1'">
                    <ul style="list-style:none; padding:0; background-color:#333333">
                        <li
                            v-for="(meat, index) in meats"
                            :key="meat"
                            class="foodBox"
                            @click="meatIndex = index; isActive = true"
                            style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;"
                        >
                            <img :src="meat.icon" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">{{ meat.name }}</h4>
                        </li>
                        <li class="foodBox"  @click="addModalIsActive = true" style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;">
                            <img src="@/assets/others/icons8-plus-100.png" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">Add</h4>
                        </li>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="isActive === true && tabNumber === '1'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="isActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <p>{{ meats[meatIndex].name }}</p>
                                    <img :src="meats[meatIndex].icon" alt="" style="width:50px; height:50px; background-color:#fff; padding:15px 50px; border-radius:50px;">
                                    <p style="margin:0 auto;">{{ meats[meatIndex].calorie }} kcal/g</p>
                                    <table border="2" bordercolor="#fff" bgcolor="#3333333" style="margin:0 auto">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td>{{ meats[meatIndex].protein }} g</td>
                                            <td>{{ meats[meatIndex].carbs }} g</td>
                                            <td>{{ meats[meatIndex].sodium }} mg</td>
                                            <td>{{ meats[meatIndex].vitamin }}% (DV)</td>
                                            <td>{{ meats[meatIndex].fats }} g</td>
                                        </tr>
                                    </table>
                                    <div class="countArea" style="display:flex; padding:5px 15%;">
                                        <button @click="meats[meatIndex].count -= 100, totalCalorieOfMeats = sumCalorieOfMeats, addCalorie">-100g</button>
                                        <button @click="meats[meatIndex].count -= 10, totalCalorieOfMeats = sumCalorieOfMeats">-10g</button>
                                        <p style="width:20%;">{{ meats[meatIndex].count }}g</p>
                                        <button @click="meats[meatIndex].count += 10, totalCalorieOfMeats = sumCalorieOfMeats">+10g</button>
                                        <button @click="meats[meatIndex].count += 100, totalCalorieOfMeats = sumCalorieOfMeats">+100g</button>
                                    </div>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="addModalIsActive === true && tabNumber === '1'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="addModalIsActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <h3 style="margin:0;">Add Food</h3>
                                    <input type="text" placeholder="Enter Food Name" v-model="newItemName" style="width:auto">
                                    <br>
                                    <input type="number" v-model="newItemCalorie" style="height:30px;">
                                    <p style="margin:0; width:0px; float:right; position:relative; left:-37%; top:3%;">kcal/g</p>
                                    <table class="addItemTable" border="2" bordercolor="#fff" bgcolor="#333333" style="margin:10px auto; width:90%;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td><input type="number" v-model="newItemProtein">g</td>
                                            <td><input type="number" v-model="newItemCarbs">g</td>
                                            <td><input type="number" v-model="newItemSodium">mg</td>
                                            <td><input type="number" v-model="newItemVitamin">% (DV)</td>
                                            <td><input type="number" v-model="newItemFats">g</td>
                                        </tr>
                                    </table>
                                    <button @click="addMeat" style="height:35px; width:65px; border:1.5px solid #333333; cursor:pointer;;">Add</button>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                    </ul>
                    </li>
                    <li v-else-if="tabNumber === '2'">
                    <ul style="list-style:none; padding:0; background-color:#333333">
                        <li
                            v-for="(vegetable, index) in vegetables"
                            :key="vegetable"
                            class="foodBox"
                            @click="vegIndex = index, isActive = true"
                            style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;"
                        >
                            <img :src="vegetable.icon" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">{{ vegetable.name }}</h4>
                        </li>
                        <li class="foodBox" @click="addModalIsActive = true" style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;">
                            <img src="@/assets/others/icons8-plus-100.png" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">Add</h4>
                        </li>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="isActive === true && tabNumber === '2'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="isActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <p>{{ vegetables[vegIndex].name }}</p>
                                    <img :src="vegetables[vegIndex].icon" alt="" style="width:50px; height:50px; background-color:#fff; padding:15px 50px; border-radius:50px;">
                                    <p style="margin:0 auto;">{{ vegetables[vegIndex].calorie }} kcal/g</p>
                                    <table class="" border="2" bordercolor="#fff" bgcolor="#333333" style="margin:0 auto; width:90%;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td>{{ vegetables[vegIndex].protein }}g</td>
                                            <td>{{ vegetables[vegIndex].carbs }}g</td>
                                            <td>{{ vegetables[vegIndex].sodium }}mg</td>
                                            <td>{{ vegetables[vegIndex].vitamin }}% (DV)</td>
                                            <td>{{ vegetables[vegIndex].fats }}g</td>
                                        </tr>
                                    </table>
                                    <div class="countArea" style="display:flex; padding:5px 15%;">
                                        <button @click="vegetables[vegIndex].count -= 100, totalCalorieOfVegetables = sumCalorieOfVegetables">-100g</button>
                                        <button @click="vegetables[vegIndex].count -= 10, totalCalorieOfVegetables = sumCalorieOfVegetables">-10g</button>
                                        <p style="width:20%;">{{ vegetables[vegIndex].count }}g</p>
                                        <button @click="vegetables[vegIndex].count += 10, totalCalorieOfVegetables = sumCalorieOfVegetables">+10g</button>
                                        <button @click="vegetables[vegIndex].count += 100, totalCalorieOfVegetables = sumCalorieOfVegetables">+100g</button>
                                    </div>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="addModalIsActive === true && tabNumber === '2'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="addModalIsActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <h3 style="margin:0;">Add Food</h3>
                                    <input type="text" placeholder="Enter Food Name" v-model="newItemName" style="width:auto">
                                    <br>
                                    <input type="number" v-model="newItemCalorie" style="height:30px;">
                                    <p style="margin:0; width:0px; float:right; position:relative; left:-37%; top:3%;">kcal/g</p>
                                    <table class="addItemTable" border="2" bordercolor="#fff" bgcolor="#333333" style="margin:10px auto; width:90%;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td><input type="number" v-model="newItemProtein">g</td>
                                            <td><input type="number" v-model="newItemCarbs">g</td>
                                            <td><input type="number" v-model="newItemSodium">mg</td>
                                            <td><input type="number" v-model="newItemVitamin">% (DV)</td>
                                            <td><input type="number" v-model="newItemFats">g</td>
                                        </tr>
                                    </table>
                                    <button @click="addVegetable" style="height:35px; width:65px; border:1.5px solid #333333; cursor:pointer;;">Add</button>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                    </ul>
                    </li>
                    <li v-else-if="tabNumber === '3'">
                        <ul style="list-style:none; padding:0; background-color:#333333">
                        <li
                            v-for="(fruit, index) in fruits"
                            :key="fruit"
                            class="foodBox"
                            @click="fruitIndex = index, isActive = true"
                            style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;"
                        >
                            <img :src="fruit.icon" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">{{ fruit.name }}</h4>
                        </li>
                        <li class="foodBox" @click="addModalIsActive = true" style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;">
                            <img src="@/assets/others/icons8-plus-100.png" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">Add</h4>
                        </li>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="isActive === true && tabNumber === '3'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="isActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <p>{{ fruits[fruitIndex].name }}</p>
                                    <img :src="fruits[fruitIndex].icon" alt="" style="width:50px; height:50px; background-color:#fff; padding:15px 50px; border-radius:50px;">
                                    <p style="margin:0 auto;">{{ fruits[fruitIndex].calorie }} kcal/g</p>
                                    <table border="2" bordercolor="#fff" bgcolor="#333333" style="margin:0 auto;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td>{{ fruits[fruitIndex].protein }}g</td>
                                            <td>{{ fruits[fruitIndex].carbs }}g</td>
                                            <td>{{ fruits[fruitIndex].sodium }}mg</td>
                                            <td>{{ fruits[fruitIndex].vitamin }}% (DV)</td>
                                            <td>{{ fruits[fruitIndex].fats }}g</td>
                                        </tr>
                                    </table>
                                    <div class="countArea" style="display:flex; padding:5px 15%;">
                                        <button @click="fruits[fruitIndex].count -= 100, totalCalorieOfFruits = sumCalorieOfFruits">-100g</button>
                                        <button @click="fruits[fruitIndex].count -= 10, totalCalorieOfFruits = sumCalorieOfFruits">-10g</button>
                                        <p style="width:20%;">{{ fruits[fruitIndex].count }}g</p>
                                        <button @click="fruits[fruitIndex].count += 10, totalCalorieOfFruits = sumCalorieOfFruits">+10g</button>
                                        <button @click="fruits[fruitIndex].count += 100, totalCalorieOfFruits = sumCalorieOfFruits">+100g</button>
                                    </div>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="addModalIsActive === true && tabNumber === '3'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="addModalIsActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <h3 style="margin:0;">Add Food</h3>
                                    <input type="text" placeholder="Enter Food Name" v-model="newItemName" style="width:auto">
                                    <br>
                                    <input type="number" v-model="newItemCalorie" style="height:30px;">
                                    <p style="margin:0; width:0px; float:right; position:relative; left:-37%; top:3%;">kcal/g</p>
                                    <table class="addItemTable" border="2" bordercolor="#fff" bgcolor="#333333" style="margin:10px auto; width:90%;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td><input type="number" v-model="newItemProtein">g</td>
                                            <td><input type="number" v-model="newItemCarbs">g</td>
                                            <td><input type="number" v-model="newItemSodium">mg</td>
                                            <td><input type="number" v-model="newItemVitamin">% (DV)</td>
                                            <td><input type="number" v-model="newItemFats">g</td>
                                        </tr>
                                    </table>
                                    <button @click="addFruit" style="height:35px; width:65px; border:1.5px solid #333333; cursor:pointer;;">Add</button>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                    </ul>
                    </li>
                    <li v-else-if="tabNumber === '4'">
                        <ul style="list-style:none; padding:0; background-color:#333333">
                        <li
                            v-for="(dairy, index) in dairies"
                            :key="dairy"
                            class="foodBox"
                            @click="dairyIndex = index, isActive = true"
                            style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;"
                        >
                            <img :src="dairy.icon" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">{{ dairy.name }}</h4>
                        </li>
                        <li class="foodBox" @click="addModalIsActive = true" style="opacity:0.95; display:inline-block; width:18.5%; height:65px; background-color:#eae7dc; border:1.5px solid #2b2b2b; margin:3px 1px; border-radius:10px; cursor:pointer;">
                            <img src="@/assets/others/icons8-plus-100.png" style="position:relative; top:10%; width:25px; height:25px; padding:5px 20px; background-color:#fff; border-radius:10px;">
                            <h4 style="margin:0 auto;">Add</h4>
                        </li>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="isActive === true && tabNumber === '4'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="isActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <p>{{ dairies[dairyIndex].name }}</p>
                                    <img :src="dairies[dairyIndex].icon" alt="" style="width:50px; height:50px; background-color:#fff; padding:15px 50px; border-radius:50px;">
                                    <p style="margin:0 auto;">{{ dairies[dairyIndex].calorie }} kcal/g</p>
                                    <table border="2" bordercolor="#fff" bgcolor="#333333" style="margin:0 auto;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td>{{ dairies[dairyIndex].protein }}g</td>
                                            <td>{{ dairies[dairyIndex].carbs }}g</td>
                                            <td>{{ dairies[dairyIndex].sodium }}mg</td>
                                            <td>{{ dairies[dairyIndex].vitamin }}% (DV)</td>
                                            <td>{{ dairies[dairyIndex].fats }}g</td>
                                        </tr>
                                    </table>
                                    <div class="countArea" style="display:flex; padding:5px 15%;">
                                        <button @click="dairies[dairyIndex].count -= 100, totalCalorieOfDairies = sumCalorieOfDairies">-100g</button>
                                        <button @click="dairies[dairyIndex].count -= 10, totalCalorieOfDairies = sumCalorieOfDairies">-10g</button>
                                        <p style="width:20%;">{{ dairies[dairyIndex].count }}g</p>
                                        <button @click="dairies[dairyIndex].count += 10, totalCalorieOfDairies = sumCalorieOfDairies">+10g</button>
                                        <button @click="dairies[dairyIndex].count += 100, totalCalorieOfDairies = sumCalorieOfDairies">+100g</button>
                                    </div>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                        <transition name="fade" mode="out-in" appear>
                            <div class="modalWrapper" v-if="addModalIsActive === true && tabNumber === '4'" style="position:fixed; top:0; left:0; z-meatIndex:100; width:100%; height:100%; background-color: rgba(0, 0, 0, 0.6)">
                                <div class="modalWindow" style="position:relative; top:20%; width:90%; height:70%; max-height:350px; background-color:#d8c3a5; margin:0 auto; border-radius:10px;">
                                    <button class="closeBtn" @click="addModalIsActive = false" style="position:fixed; right:5%;border-radius:0 10px 0 0; margin:0 0 0 auto; cursor:pointer;">X</button>
                                    <h3 style="margin:0;">Add Food</h3>
                                    <input type="text" placeholder="Enter Food Name" v-model="newItemName" style="width:auto">
                                    <br>
                                    <input type="number" v-model="newItemCalorie" style="height:30px;">
                                    <p style="margin:0; width:0px; float:right; position:relative; left:-37%; top:3%;">kcal/g</p>
                                    <table class="addItemTable" border="2" bordercolor="#fff" bgcolor="#333333" style="margin:10px auto; width:90%;">
                                        <tr>
                                            <th>Protein</th>
                                            <th>Carbs</th>
                                            <th>Sodium</th>
                                            <th>Vitamin</th>
                                            <th>Fats</th>
                                        </tr>
                                        <tr>
                                            <td><input type="number" v-model="newItemProtein">g</td>
                                            <td><input type="number" v-model="newItemCarbs">g</td>
                                            <td><input type="number" v-model="newItemSodium">mg</td>
                                            <td><input type="number" v-model="newItemVitamin">% (DV)</td>
                                            <td><input type="number" v-model="newItemFats">g</td>
                                        </tr>
                                    </table>
                                    <button @click="addDairy" style="height:35px; width:65px; border:1.5px solid #333333; cursor:pointer;;">Add</button>
                                    <h3>Total: {{ totalCalories }} kcal</h3>
                                </div>
                            </div>
                        </transition>
                    </ul>
                    </li>
                </ul>
            </div>
        </div>
        <div class="footer" style="background-color:#333333">
            <ul class="footerMenu" style="float:right; margin:0; padding-right:10px;">
                <li>Contact Us</li>
                <li>Terms of Use</li>
            </ul>
            <p style="padding-left:10px;">Nutrient</p>
            <p style="padding-left:10px;">& Calorie Checker</p>
        </div>
    </div>
</template>

<script>
export default {
  components: {
  },
  data() {
    return {
        date: '',
        targetCalories: 3000,
        tabNumber: '1',
        newItemName: '',
        newItemCalorie: 0,
        newItemProtein: 0,
        newItemCarbs: 0,
        newItemSodium: 0,
        newItemFats: 0,
        newItemVitamin: 0,
        meats: [
            {name: 'Chicken', icon: require('@/assets/meat-icons/icons8-chicken-100.png'), calorie: 2.29, protein: 0.25, carbs: 0, sodium: 0, vitamin: 0.01, fats: 0.13, count: 0},
            {name: 'Beef', icon: require('@/assets/meat-icons/cow.png'), calorie: 2.98, protein: 0.17, carbs: 0.4, sodium: 0, vitamin: 0, fats: 0.24, count: 0},
            {name: 'Pork', icon: require('@/assets/meat-icons/icons8-pork-64.png'), calorie: 3.86, protein: 0.14, carbs: 0, sodium: 0, vitamin: 0, fats: 0.35, count: 0},
            {name: 'Lamb', icon: require('@/assets/meat-icons/icons8-sheep-64.png'), calorie: 2.27, protein: 0.18, carbs: 0, sodium: 0, vitamin: 0, fats: 0.16, count: 0},
            {name: 'Goat', icon: require('@/assets/meat-icons/goat.png'), calorie: 1.09, protein: 0.2, carbs: 0, sodium: 0, vitamin: 0, fats: 0.02, count: 0},
        ],
        vegetables: [
            {name: 'Carrot', icon: require('@/assets/vegetable-icons/icons8-carrot-100.png'), calorie: 0.44, protein: 0.02, carbs: 0.1, sodium: 0.11, vitamin: 0.03, fats: 0, count: 0},
            {name: 'Tomato', icon: require('@/assets/vegetable-icons/icons8-tomato-100.png'), calorie: 0.18, protein: 0.01, carbs: 0.05, sodium: 0.03, vitamin: 0.28, fats: 0, count: 0},
            {name: 'Cucumber', icon: require('@/assets/vegetable-icons/icons8-cucumber-100.png'), calorie: 0.15, protein: 0.01, carbs: 0.04, sodium: 0.02, vitamin: 0.05, fats: 0, count: 0},
            {name: 'Corn', icon: require('@/assets/vegetable-icons/icons8-corn-100.png'), calorie: 0.96, protein: 0.03, carbs: 0.2, sodium: 0.01, vitamin: 0.01, fats: 0.16, count: 0},
            {name: 'Pumpkin', icon: require('@/assets/vegetable-icons/icons8-pumpkin-100.png'), calorie: 0.2, protein: 0.01, carbs: 0.05, sodium: 0.01, vitamin: 0.08, fats: 0.02, count: 0},
        ],
        fruits: [
            {name: 'Apple', icon: require('@/assets/fruit-icons/icons8-apple-100.png'), calorie: 0.52, protein: 0, carbs: 0.14, sodium: 0.01, vitamin: 0.08, fats: 0, count: 0},
            {name: 'Banana', icon: require('@/assets/fruit-icons/icons8-banana-60.png'), calorie: 0.9, protein: 0.01, carbs: 0.23, sodium: 0.01, vitamin: 0.14, fats: 0, count: 0},
            {name: 'Cherry', icon: require('@/assets/fruit-icons/icons8-cherry-100.png'), calorie: 0.63, protein: 0.01, carbs: 0.16, sodium: 0, vitamin: 0.12, fats: 0, count: 0},
            {name: 'Grapes', icon: require('@/assets/fruit-icons/icons8-grapes-64.png'), calorie: 0.7, protein: 0.01, carbs: 0.26, sodium: 0.03, vitamin: 0.05, fats: 0, count: 0},
            {name: 'Kiwi', icon: require('@/assets/fruit-icons/icons8-kiwi-100.png'), calorie: 0.6, protein: 0.01, carbs: 0.14, sodium: 0.03, vitamin: 1.55, fats: 0, count: 0},
        ],
        dairies: [
            {name: 'Butter', icon: require('@/assets/dairy-icons/butter.png'), calorie: 7.29, protein: 0.01, carbs: 0, sodium: 6.5, vitamin: 0, fats: 0.86, count: 0},
            {name: 'Cheese', icon: require('@/assets/dairy-icons/icons8-cheese-100.png'), calorie: 4.04, protein: 0.23, carbs: 0.03, sodium: 6.5, vitamin: 0, fats: 0.33, count: 0},
            {name: 'Egg', icon: require('@/assets/dairy-icons/icons8-eggs-100.png'), calorie: 1.44, protein: 0.12, carbs: 0.01, sodium: 1.42, vitamin: 0, fats: 0.1, count: 0},
            {name: 'Milk', icon: require('@/assets/dairy-icons/icons8-milk-carton-100.png'), calorie: 0.5, protein: 0.03, carbs: 0.05, sodium: 0.47, vitamin: 0, fats: 0.02, count: 0},
            {name: 'Yogurt', icon: require('@/assets/dairy-icons/yogurt.png'), calorie: 0.63, protein: 0.05, carbs: 0.07, sodium: 0.7, vitamin: 0.01, fats: 0.02, count: 0},
        ],
        isOpen: false,
        isActive: false,
        addModalIsActive: false,
        hover: false,
        totalCalorieOfMeats: 0,
        totalCalorieOfVegetables: 0,
        totalCalorieOfFruits: 0,
        totalCalorieOfDairies: 0
    }
  },
  methods: {
    printDate() {
        return new Date().toLocaleDateString();
    },
    changeTab(number) {
        this.tabNumber = number
    },
    resetModalInfo() {
        this.newItemName = ''
        this.newItemCalorie = 0
        this.newItemProtein = 0
        this.newItemCarbs = 0
        this.newItemSodium = 0
        this.newItemFats = 0
        this.newItemVitamin = 0
    },
    addMeat() {
        if(this.newItemName != '') {
            this.meats.push({name: this.newItemName, icon: require('@/assets/meat-icons/icons8-meat-100.png'), calorie: this.newItemCalorie, protein: this.newItemProtein, carbs: this.newItemCarbs, sodium: this.newItemSodium, vitamin: this.newItemVitamin, fats: this.newItemFats, count: 0});
            this.addModalIsActive = false
            this.resetModalInfo();
        }
    },
    addVegetable() {
        if(this.newItemName != '') {
            this.vegetables.push({name: this.newItemName, icon: require('@/assets/vegetable-icons/icons8-vegetable-64.png'), calorie: this.newItemCalorie, protein: this.newItemProtein, carbs: this.newItemCarbs, sodium: this.newItemSodium, vitamin: this.newItemVitamin, fats: this.newItemFats, count: 0});
            this.addModalIsActive = false
            this.resetModalInfo();
        }
    },
    addFruit() {
        if(this.newItemName != '') {
            this.fruits.push({name: this.newItemName, icon: require('@/assets/fruit-icons/icons8-fruits-64.png'), calorie: this.newItemCalorie, protein: this.newItemProtein, carbs: this.newItemCarbs, sodium: this.newItemSodium, vitamin: this.newItemVitamin, fats: this.newItemFats, count: 0});
            this.addModalIsActive = false
            this.resetModalInfo();
        }
    },
    addDairy() {
        if(this.newItemName != '') {
            this.dairies.push({name: this.newItemName, icon: require('@/assets/dairy-icons/icons8-dairy-products-68.png'), calorie: this.newItemCalorie, protein: this.newItemProtein, carbs: this.newItemCarbs, sodium: this.newItemSodium, vitamin: this.newItemVitamin, fats: this.newItemFats, count: 0});
            this.addModalIsActive = false
            this.resetModalInfo();
        }
    }
  },
  computed: {
    currentAngle() {
        return Math.floor(360 * this.totalCalories / this.targetCalories);
    },
    rightAngle(){
        let angle = Math.min(this.currentAngle, 180);
        return {
            "transform": "rotate(" + angle + "deg)",
        }
    },
    leftAngle(){
        let angle = Math.min(Math.max(this.currentAngle-180, 0),180);
        return {
            "transform": "rotate(" + angle + "deg)",
        }
    },
    sumCalorieOfMeats() {
        let totalCalorieOfMeats = 0;
        for(let i = 0; i < this.meats.length; i++) {
            totalCalorieOfMeats += this.meats[i].calorie * this.meats[i].count;
        }
        return Math.round(totalCalorieOfMeats * 10) / 10;
    },
    sumCalorieOfVegetables() {
        let totalCalorieOfVegetables = 0;
        for(let i = 0; i < this.vegetables.length; i++) {
            totalCalorieOfVegetables += this.vegetables[i].calorie * this.vegetables[i].count;
        }
        return Math.round(totalCalorieOfVegetables * 10) / 10;
    },
    sumCalorieOfFruits() {
        let totalCalorieOfFruits = 0;
        for(let i = 0; i < this.fruits.length; i++) {
            totalCalorieOfFruits += this.fruits[i].calorie * this.fruits[i].count;
        }
        return Math.round(totalCalorieOfFruits * 10) / 10;
    },
    sumCalorieOfDairies() {
        let totalCalorieOfDairies = 0;
        for(let i = 0; i < this.dairies.length; i++) {
            totalCalorieOfDairies += this.dairies[i].calorie * this.dairies[i].count;
        }
        return Math.round(totalCalorieOfDairies * 10) / 10;
    },
    totalCalories() {
        let totalCalories = 0;
        totalCalories = this.totalCalorieOfMeats + this.totalCalorieOfVegetables + this.totalCalorieOfFruits + this.totalCalorieOfDairies;
        return Math.round(totalCalories * 10) / 10;
    },
    totalProtein() {
        let totalProtein = 0;
        for(let i = 0; i < this.meats.length; i++) {
            totalProtein += this.meats[i].protein * this.meats[i].count;
        }
        for(let i = 0; i < this.vegetables.length; i++) {
            totalProtein += this.vegetables[i].protein * this.vegetables[i].count;
        }
        for(let i = 0; i < this.fruits.length; i++) {
            totalProtein += this.fruits[i].protein * this.fruits[i].count;
        }
        for(let i = 0; i < this.dairies.length; i++) {
            totalProtein += this.dairies[i].protein * this.dairies[i].count;
        }
        return Math.round(totalProtein * 10) / 10;
    },
    totalCarbs() {
        let totalCarbs = 0;
        for(let i = 0; i < this.meats.length; i++) {
            totalCarbs += this.meats[i].carbs * this.meats[i].count;
        }
        for(let i = 0; i < this.vegetables.length; i++) {
            totalCarbs += this.vegetables[i].carbs * this.vegetables[i].count;
        }
        for(let i = 0; i < this.fruits.length; i++) {
            totalCarbs += this.fruits[i].carbs * this.fruits[i].count;
        }
        for(let i = 0; i < this.dairies.length; i++) {
            totalCarbs += this.dairies[i].carbs * this.dairies[i].count;
        }
        return Math.round(totalCarbs * 10) / 10;
    },
    totalSodium() {
        let totalSodium = 0;
        for(let i = 0; i < this.meats.length; i++) {
            totalSodium += this.meats[i].sodium * this.meats[i].count;
        }
        for(let i = 0; i < this.vegetables.length; i++) {
            totalSodium += this.vegetables[i].sodium * this.vegetables[i].count;
        }
        for(let i = 0; i < this.fruits.length; i++) {
            totalSodium += this.fruits[i].sodium * this.fruits[i].count;
        }
        for(let i = 0; i < this.dairies.length; i++) {
            totalSodium += this.dairies[i].sodium * this.dairies[i].count;
        }
        return Math.round(totalSodium * 10) / 10;
    },
    totalVitamin() {
        let totalVitamin = 0;
        for(let i = 0; i < this.meats.length; i++) {
            totalVitamin += this.meats[i].vitamin * this.meats[i].count;
        }
        for(let i = 0; i < this.vegetables.length; i++) {
            totalVitamin += this.vegetables[i].vitamin * this.vegetables[i].count;
        }
        for(let i = 0; i < this.fruits.length; i++) {
            totalVitamin += this.fruits[i].vitamin * this.fruits[i].count;
        }
        for(let i = 0; i < this.dairies.length; i++) {
            totalVitamin += this.dairies[i].vitamin * this.dairies[i].count;
        }
        return Math.round(totalVitamin * 10) / 10;
    },
    totalFats() {
        let totalFats = 0;
        for(let i = 0; i < this.meats.length; i++) {
            totalFats += this.meats[i].fats * this.meats[i].count;
        }
        for(let i = 0; i < this.vegetables.length; i++) {
            totalFats += this.vegetables[i].fats * this.vegetables[i].count;
        }
        for(let i = 0; i < this.fruits.length; i++) {
            totalFats += this.fruits[i].fats * this.fruits[i].count;
        }
        for(let i = 0; i < this.dairies.length; i++) {
            totalFats += this.dairies[i].fats * this.dairies[i].count;
        }
        return Math.round(totalFats * 10) / 10;
    }
  },
    mounted: function() {
        this.date = this.printDate();
    }
}
</script>

<style scoped>
    @media screen and (max-width: 480px) {
        td {
            font-size: 11px !important;
        }
        h4 {
            font-size: 12px;
        }
        input {
            padding-left: 5px !important;
        }
        .nutrientBoards {
            width:95vw !important;
        }
        .boardLeft {
            padding-right: 14% !important;
            padding-left: 1% !important;
        }
        .boardRight {
            padding-left: 14%;
            padding-right: 1%;
        }
        .boxRight {
        margin-right: -15% !important;
        }
        .tabButton {
            font-size: 16px !important;
        }
        .countArea {
            padding: 5px 5% !important;
        }
    }

    ul {
        list-style: none;
    }

    th, td {
        width: 20%;
        font-size: 13px;
        padding: 3px;
    }

    input {
        text-align: center;
        height: 50px;
        width: 80px;
        opacity: 0.8;
        box-sizing: border-box;
        border-radius: 20px;
        border: 2px solid gray;
        padding-left: 20px;
        margin: 5px 10px;
    }

    button {
        border: none;
        border-radius: 5px;
    }

    .header {
        height: 30px;
        width: 100%;
        padding: 5px 0px;
        background-color: #333333;
        color: #fff;
        display: flex;
    }

    .menuBtn span {
        display: inline-block;
        position: relative;
        right: 5%;
        height: 3px;
        width: 20px;
        background-color: #333333;
        border-radius: 5px;
        transition: all .5s;
    }

    .menuBtn.open span:nth-of-type(1) {
        top: -1px !important;
        left: 11px;
        background: #ff6666;
        transform: translateY(6px) rotate(-50deg);
    }

    .menuBtn.open span:nth-of-type(2) {
        opacity: 0;
    }

    .menuBtn.open span:nth-of-type(3) {
        top: -10px !important;
        left: 7px;
        background: #ff6666;
        transform: translate(-6px) rotate(50deg);
    }

    .menuWindow li {
        text-align: center;
        padding: 15px;
        box-sizing: border-box;
        border: 1px solid #dedede;
    }

    .container {
        width: 100%;
        padding-right: 0 auto;
        padding: 10px 0px;
        background-color: #eae7dc;
    }

    .footer {
        height: 200px;
        width: 100%;
        padding: 10px 0px;
    }

    .footer p, li {
        color: #fff;
        margin: 0;
    }

    /* Nutrient Boards */
    .board {
        width: 50%;
        height: 100%;
        float: left;
        text-align: center;
    }

    .board p {
        margin: 0 auto;
    }

    .nutrientBox {
        color: #fff;
        font-size: 16px;
        font-weight: bold;
        list-style-type: none;
        width: 70%;
        height: 31.5%;
        line-height: 32px;
        margin: 0 auto 3.5px auto;
        background-color: #333333;
        border: 1.5px solid #fff;
        border-radius: 10px;
        box-shadow: 5px 5px 5px darkgray;
    }

    .boxRight {
        margin-right: 0%;
    }

    .boxLeft {
        margin-left: 0%;;
    }

    .amount {
        color: #fff;
        height: 31px;
        width: 90%;
        border: 1.5px solid #fff;
        border-radius: 20px;
        margin: 0 auto;
    }

    /* Calorie Check form */
    .calorieCheckForm {
        color: lightseagreen;
        position: relative;
        margin: 0 auto;
        text-align: center;
    }

    .square{
        position:absolute;
        width: 100px;
        height :200px;
        overflow: hidden;
    }

    .circle {
        position: absolute;
        width: 200px;
        height: 200px;
        border-radius: 50%;
        border:10px solid gray;
        box-sizing: border-box;
    }

    .circleRed {
        border:10px solid lightseagreen;
    }

    .gray {
        color: gray;
    }

    .calorieCheckForm input[type='number'] {
        text-align: center;
        height: 50px;
        width: 80px;
        opacity: 0.8;
        box-sizing: border-box;
        border-radius: 20px;
        border: 2px solid gray;
        margin-bottom: 10px;
        padding-left: 20px;
    }

    /* Selected Item Area */
    .selectedItemArea th {
        width: 25%;
        border: 1.5px solid;
        border-radius: 5px;
    }

    .selectedItemArea td {
        height: 50px;
        border: 1.5px solid;
        border-radius: 5px;
    }

    .itemBlock {
        width: 50%;list-style: none;
        float: left;
        margin: 5px 0;
        text-align: center;
    }

    .selectedItemArea img {
        width: 70%;
        padding: 1% 10%;
        background-color: #dedede;
        border-radius: 10px;
        border: 1.5px solid #fff;
    }

    .selectedItemArea h5 {
        text-align: center;
        margin: -5px auto;
    }

    /* Food Display */
    .foodDisplay {
        margin-top: 30px;
        text-align: center;
        color: #fff;
        padding: 0 1%; 
    }

    .foodTab {
        list-style-type: none;
        padding-inline-start: 0;
        margin: 20px 0 0 0;
        cursor: pointer;
    }

    .tabButton {
        background-color: #d8c3a5;
        display: inline-block;
        width: 25%;
        height: 30px;
        font-size: 18px;
        border: 1.5px solid #333333;
        border-bottom: none;
        border-radius: 8px 8px 0 0;
        box-sizing: border-box;
    }

    .active {
        background-color: #333333;
        transition: width 225ms ease, left 225ms;
        will-change: width, left;
        animation-name: displayAnime;
        animation-duration: 1.5s;
        animation-fill-mode: forwards;
    }

    /* Food List */
    .foodBox {
        box-shadow: 1.5px 1.5px 0px gray;
    }

    .foodBox:hover {
        background: #d8c3a5 !important;
        transition: .3s ease-in-out;
    }

    .foodBox:active {
        box-shadow: none;
        transform: translateY(2px);
        transition: .1s all;
    }

    /* Modal Window */
    .closeBtn {
        padding: 5px 10px;
    }

    .closeBtn:hover {
        color: #fff;
        background-color: #c2302a;

        transition: .3s ease-in-out;
    }

    .countArea button {
        border-radius: 10px;
        width: 20%;
        height: 40px;
        margin: auto 3px;
        border: 1.5px solid #333333
    }

    .addItemTable input {
        width: 90%;
        height: 30px;
        margin: 0 5px;
    }

    /* fade */
    .fade-enter-from {
        opacity: 0;
    }

    .fade-enter-active, .fade-leave-active {
        transition: opacity 0.5s;
    }

    .fade-enter-to {
        opacity: 1;
    }

    .fade-leave-from {
        opacity: 1;
    }

    .fade-leave-to {
        opacity: 0;
    }

    /* Animation */
    @keyframes displayAnime {
        from {
            opacity: 0;
        } to {
            opacity: 1;
        }
    }
</style>
