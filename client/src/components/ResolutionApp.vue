<template>
  <div id="ResolutionApp" class="ResolutionApp">
    <div id="EmptySpace" class="EmptySpace"></div>
    <div id="RAWrapper" class="RAWrapper">
      <div id="MSG" class="MSG">
        {{ msg }}
      </div>
      <div id="LeftSide" class="LeftSide">
        <div id="SelectBoxArea" class="SelectBoxArea">
          <h3>
            Select Ratio
          </h3>
          <p>
            Resolution can be calculated by ratio that is being selected.
          </p>
          <p>
            Custom ratio option is provided.
          </p>
        <select id="ResolutionSelectBox" class="ResolutionSelectBox" v-model="SelectValue" @change="getRatio">
          <option value="ratio" disabled="disabled" selected="selected">Ratio</option>
          <option value="custom">Custom</option>
          <option value="3:4">3:4</option>
          <option value="4:3">4:3</option>
          <option value="5:3">5:3</option>
          <option value="5:4">5:4</option>
          <option value="16:9">16:9</option>
          <option value="16:10">16:10</option>
          <option value="17:10">17:10</option>
        </select>
      </div>

        <div id="RatioInput" class="RatioInput">
          <div>
            <h3>
              Width Ratio X Height Ratio
            </h3>
            <input class="WidthRatio" id="WidthRatio" name="WidthRatio" type="text" v-model="WidthRatio" @blur="updateList" /> x <input class="HeightRatio" id="HeightRatio" name="HeightRatio" type="text" v-model="HeightRatio" @blur="updateList" />
          </div>

          <div>
            <h3>
              Width X Height to Ratio
            </h3>
            <p>
              The ratio will be updated above.
            </p>
            <input class="ToRatioWidth" id="ToRatioWidth" name="ToRatioWidth" type="text" v-model="ToRatioWidth" @blur="toRatio" /> x <input class="ToRatioHeight" id="ToRatioHeight" name="ToRatioHeight" type="text" v-model="ToRatioHeight" @blur="toRatio" />
          </div>
        </div>

        <div id="Settings" class="Settings">
          <h3>
            Other Settings
          </h3>
          <div>
            <h4>
              Max height of Resolution List
            </h4>
            <input class="MaxHeight" id="MaxHeight" name="MaxHeight" type="text" v-model="MaxHeight" @blur="updateList" />
          </div>
        </div>

      </div>

      <div id="RightSide" class="RightSide">
        <div id="ResolutionInput" class="ResolutionInput">
          <div>
            <h3>
              Width X Height
            </h3>
          </div>
          <input class="Width" id="Width" name="Width" type="text" v-model="Width" @blur="calculateInputHeight" /> x <input class="Height" id="Height" name="Height" type="text" v-model="Height" @blur="calculateInputWidth" />
        </div>
        <div>
          <h3>
            Resolution List
          </h3>
          <textarea class="ResolutionList" id="ResolutionList" name="ResolutionList" v-model="ResolutionList" />
        </div>
      </div>

    </div>
  </div>
</template>

<script>

export default {
  name: 'ResolutionApp',
  data: function() {
    return {
      SelectValue: 'ratio',
      WidthRatio: '',
      HeightRatio: '',
      ToRatioWidth: '',
      ToRatioHeight: '',
      Width: '',
      Height: '',
      ResolutionList: '',
      MaxHeight: 5000,
      msg: ''
    }
  },
  computed: {
  },
  methods: {
    getRatio: function () {
      this.msg = "";
      if(this.SelectValue != "ratio" && this.SelectValue != "custom" ) {
        var ratioVals = this.SelectValue.split(':');
        this.WidthRatio = ratioVals[0];
        this.HeightRatio = ratioVals[1];
        this.updateList();
      }
    },
    updateList: function() {
      this.msg = "";
      if(this.SelectValue == "ratio") {
        this.msg = "Error: It is not allowed to update a resolution list when ratio option is not selected from select box. Please select an option.";
      }
      else if(this.SelectValue == "custom"){
        if(isNaN(this.WidthRatio) || isNaN(this.HeightRatio)){
          this.msg = "Error: Ratio should be number.";
        }
        else {
          this.calculateList();
        }
      }
      else {
        var ratioVals = this.SelectValue.split(':');
        if(ratioVals[0] != this.WidthRatio || ratioVals[1] != this.HeightRatio) {
            this.msg = "Error: The selected option and ratio is different";
        }
        else {
          this.calculateList();
        }
      }
    },
    calculateList: function() {
      var NewResolutionList = "";
      for(var i = 10; i < this.MaxHeight; i+= 10) {
        var crw = i * this.WidthRatio / this.HeightRatio;
        var crh = i;
        var isCrwEndWithZero = crw%10;
        if(isCrwEndWithZero==0){
          NewResolutionList = NewResolutionList + crw + " x " + crh + "\n";
        }
      }
      this.ResolutionList = NewResolutionList;
    },
    calculateInputWidth: function() {
      if(isNaN(this.WidthRatio) || isNaN(this.HeightRatio) || this.WidthRatio == '' || this.HeightRatio == ''){
        this.msg = "Error: Please setup the ratio.";
      }
      else{
        if(isNaN(this.Width) || isNaN(this.Height)){
          this.msg = "Error: Width and Height input should be a number.";
        }
        else {
          this.Width = this.Height * this.WidthRatio / this.HeightRatio;
        }
      }
    },
    calculateInputHeight: function() {
      if(isNaN(this.WidthRatio) || isNaN(this.HeightRatio) || this.WidthRatio == '' || this.HeightRatio == ''){
        this.msg = "Error: Please setup the ratio.";
      }
      else{
        if(isNaN(this.Width) || isNaN(this.Height)){
          this.msg = "Error: Width and Height input should be a number.";
        }
        else {
          this.Height = this.Width * this.HeightRatio / this.WidthRatio;
        }
      }
    },
    getPrimes: function(max){
      var sieve = [], i, j, primes = [];
      for (i = 2; i <= max; ++i) {
        if (!sieve[i]) {
          primes.push(i);
          for (j = i << 1; j <= max; j += i) {
            sieve[j] = true;
          }
        }
      }
      return primes;
    },
    remove: function(array, element){
      const index = array.indexOf(element);
      if (index !== -1) {
        array.splice(index, 1);
      }
    },
    toRatio: function(){
      if(isNaN(this.ToRatioWidth) || isNaN(this.ToRatioHeight) || this.ToRatioWidth == '' || this.ToRatioHeight == ''){
        this.msg = "Error: The input should be a number."
      } else {
        this.msg = "Calculating..."
        var width = this.ToRatioWidth;
        var height = this.ToRatioHeight;
        var primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997];
        var size = primes.length;
        while(size > 0){
          for(var i = primes.length - 1; i > -1; i--){
            var tempWidth = width % primes[i];
            var tempHeight = height % primes[i];
            if(tempWidth == 0 && tempHeight == 0) {
              width = width / primes[i];
              height = height / primes[i];
            } else {
              this.remove(primes, primes[i]);
              size--;
            }
          }
        }
        this.msg = "Calculation done!..";
        this.WidthRatio = width;
        this.HeightRatio = height;
        this.SelectValue = "custom";
        this.updateList();
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#ResolutionApp {
  min-height: 100%;
}

#RAWrapper {
  margin:  10px;
  min-height: calc(100% - 22px);
}

#LeftSide,
#RightSide {
  vertical-align: top;
  width: 50%;
  display: inline-block;
  margin: 0px auto 20px auto;
}

#ResolutionList {
  height: 400px;
  width: calc(100% - 20px);
  background-color: #383c44;
  color: #cccccc;
  resize: none;
}

#ResolutionSelectBox {
  width: 50%;
  height: 34px;
  background-color: #383c44;
  color: #cccccc;
}

#MSG {
  height: 40px;
  padding: 10px;
  margin: 10px;
  background-color: #222222;
  border: 1px solid #111111;
}

input {
  height: 28px;
  width: 45%;
  background-color: #383c44;
  color: #cccccc;
  text-align: center;
}
</style>
