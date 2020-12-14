<template>


  <div class="hasmetamask">
    <div class="popup" id="popup">
      <div class="popup-inner">


        <div class="sale">
          <img src="./assets/img/interface_final_open.png" alt="" width="100%" class="sale">
        </div>
      </div>

      <div class="refreshButton" v-if="!this.userAddress.startsWith('0x')" v-on:click="reloadSale()"><img src="./assets/img/reload.png" width="32px"> </div>

      <div class="initMessage" v-if="this.userAddress.startsWith('0x')" v-on:click="reloadSale()">
        Click a button, <div><img src="./assets/img/hexagon_blank.png" width="50px"></div> from the menu below.
      </div>

      <div class="balance" v-if="!this.userAddress.startsWith('0x')">
        <div id="balance_num" style="display: inline-block;">Connect to Metamask...<br>Then, press reload.</div>
      </div>

      <div class="txtform"><input type="text" id="tbox" name="tbox" placeholder="Enter amount"></div>
      <div class="sendButton" style="display: none">Send</div>
      <div class="resetButton" style="display: none">Reset</div>
      <div class="textField" style="display: none"><img src="./assets/img/textField.png"></div>
      <div class="arrows"><img src="./assets/img/arrows.png" width="40px"></div>

      <div class="blankScreen" v-if="!this.userAddress.startsWith('0x')">
        <img src="./assets/img/interface_final_closed.png" width="440px">
      </div>

      <div class="ethButton"><img src="./assets/img/ethButton.png" width="71px"></div>
      <div class="krkButton"><img src="./assets/img/krkButton.png" width="71px"></div>
      <div class="ethWalletButton"><img src="./assets/img/ethWalletButton.png" width="71px"></div>
      <div class="krkWalletButton"><img src="./assets/img/krkWalletButton.png" width="71px"></div>
      <div class="rewardButton"><img src="./assets/img/rewardButton.png" width="71px"></div>

      <div class="ethButtonBlue"><img src="./assets/img/ethButtonBlue.png" width="71px"></div>
      <div class="krkButtonBlue"><img src="./assets/img/krkButtonBlue.png" width="71px"></div>
      <div class="ethWalletButtonBlue"><img src="./assets/img/ethWalletButtonBlue.png" width="71px"></div>
      <div class="krkWalletButtonBlue"><img src="./assets/img/krkWalletButtonBlue.png" width="71px"></div>
      <div class="rewardButtonBlue"><img src="./assets/img/rewardButtonBlue.png" width="71px"></div>

      <div class="ethField" v-on:mouseenter="ethButtonBlueShow" v-on:mouseleave="ethButtonBlueHide" v-on:click="ethButtonBlueClick"> </div>
      <div class="krkField" v-on:mouseenter="krkButtonBlueShow" v-on:mouseleave="krkButtonBlueHide" v-on:click="krkButtonBlueClick"> </div>
      <div class="ethWalletField" v-on:mouseenter="ethWalletButtonBlueShow" v-on:mouseleave="ethWalletButtonBlueHide" v-on:click="ethWalletButtonBlueClick"> </div>
      <div class="krkWalletField" v-on:mouseenter="krkWalletButtonBlueShow" v-on:mouseleave="krkWalletButtonBlueHide" v-on:click="krkWalletButtonBlueClick"> </div>
      <div class="rewardField" v-on:mouseenter="rewardButtonBlueShow" v-on:mouseleave="rewardButtonBlueHide" v-on:click="rewardButtonBlueClick"> </div>


    </div>
  </div>


</template>

<script>
import web3 from '../contracts/web3';
// import auctionBox from '../contracts/auctionBoxInstance';






// function numberWithCommas(x) {
//   return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
// }
//
// function truncateNumber(x) {
//   return Math.trunc(x * 1000) / 1000;
// }
//
// function formatNumber(x) {
//   return x.replace(/,/g, '');
// }

function formatAddress(address) {
  return address.substr(0, 5) + "..." + address.substr(35);
}


export default {
  name: 'APP',
  created: function () {
    this.interval = setInterval(() => this.refreshData(), 3000);
  },
  data: function () {
    return {
      ethButtonClicked:false,
      krkButtonClicked:false,
      ethWalletButtonClicked:false,
      krkWalletButtonClicked:false,
      rewardButtonClicked:false,

      userAddress: 'Connect to Metamask, then reload.'

    };
  },
  beforeMount() {
    this.userAddress = formatAddress(web3.eth.accounts.givenProvider.selectedAddress);
    if(this.userAddress.startsWith('0x')) document.getElementsByClassName('blankScreen')[0].style.display = "none";
  },
  methods: {
// ----------------BUTTONS START---------------
    ethButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(!this.ethButtonClicked) document.getElementsByClassName('ethButtonBlue')[0].style.display = "block";
      },
    ethButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      document.getElementsByClassName('ethButtonBlue')[0].style.display = "none";
      },
    ethButtonBlueClick(){
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.ethButtonClicked = true;
      this.grayOutButtonsExcept('ethButton');
      this.enableTextField();
    },
    krkButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(!this.krkButtonClicked) document.getElementsByClassName('krkButtonBlue')[0].style.display = "block";
    },
    krkButtonBlueHide(){
      document.getElementsByClassName('krkButtonBlue')[0].style.display = "none";
    },
    krkButtonBlueClick(){
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.krkButtonClicked = true;
      this.grayOutButtonsExcept('krkButton');
      this.enableTextField();
    },
    ethWalletButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(!this.ethWalletButtonClicked) document.getElementsByClassName('ethWalletButtonBlue')[0].style.display = "block";
    },
    ethWalletButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      document.getElementsByClassName('ethWalletButtonBlue')[0].style.display = "none";
    },
    ethWalletButtonBlueClick(){
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.ethWalletButtonClicked = true;
      this.grayOutButtonsExcept('ethWalletButton');
      this.disableTextField();
    },

    krkWalletButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(!this.krkWalletButtonClicked) document.getElementsByClassName('krkWalletButtonBlue')[0].style.display = "block";
    },
    krkWalletButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      document.getElementsByClassName('krkWalletButtonBlue')[0].style.display = "none";
    },
    krkWalletButtonBlueClick(){
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.krkWalletButtonClicked = true;
      this.grayOutButtonsExcept('krkWalletButton');
      this.disableTextField();
    },
    rewardButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(!this.rewardButtonClicked) document.getElementsByClassName('rewardButtonBlue')[0].style.display = "block";
    },
    rewardButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      document.getElementsByClassName('rewardButtonBlue')[0].style.display = "none";
    },
    rewardButtonBlueClick(){
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.rewardButtonClicked = true;
      this.grayOutButtonsExcept('rewardButton');
      this.disableTextField();
    },
    resetButtons(){
      this.ethButtonClicked = false;
      this.krkButtonClicked = false;
      this.ethWalletButtonClicked = false;
      this.krkWalletButtonClicked = false;
      this.rewardButtonClicked = false;
    },
    grayOutButtonsExcept(bttnExcpt){
      document.getElementsByClassName('ethButton')[0].style.display = "block";
      document.getElementsByClassName('krkButton')[0].style.display = "block";
      document.getElementsByClassName('ethWalletButton')[0].style.display = "block";
      document.getElementsByClassName('krkWalletButton')[0].style.display = "block";
      document.getElementsByClassName('rewardButton')[0].style.display = "block";
      document.getElementsByClassName(bttnExcpt)[0].style.display = "none";
    },

    disableTextField(){
      document.getElementsByClassName('ethWalletButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('initMessage')[0].style.display = "none";
      document.getElementsByClassName('txtform')[0].style.display = "none";
      document.getElementsByClassName('sendButton')[0].style.display = "none";
      document.getElementsByClassName('resetButton')[0].style.display = "none";
      document.getElementsByClassName('textField')[0].style.display = "none";
    },
    enableTextField(){
      document.getElementsByClassName('krkButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('initMessage')[0].style.display = "none";
      document.getElementsByClassName('txtform')[0].style.display = "block";
      document.getElementsByClassName('sendButton')[0].style.display = "block";
      document.getElementsByClassName('resetButton')[0].style.display = "block";
      document.getElementsByClassName('textField')[0].style.display = "block";
    },
// ----------------BUTTONS END---------------














    reloadSale(){window.location.reload(true);},



  },
};
</script>

<style>
body {background:none transparent !important;
}

#tbox{
  width: 160px !important;
  outline: none;
}


/*----------------BUTTONS START-----------------------------------*/
.arrows{
  position: absolute;
  margin-top: 142px;
  margin-right: -1px;
  z-index: 2;
}


/*---------------------------------------------------*/

.refreshButton {
  position: absolute;
  margin-top: -313px;
  margin-left: 339px;
  cursor: pointer;
  z-index: 10;
}

.initMessage{
  position: absolute;
  margin-bottom: 195px;
  text-align: center;
}

.ethButton{
  position: absolute;
  margin-top: 141px;
  margin-right: 74px;
}

.krkButton{
  position: absolute;
  margin-top: 142px;
  margin-left: 75px;
}

.ethWalletButton{
  position: absolute;
  margin-top: 205px;
  margin-right: 184px;
}

.krkWalletButton{
  position: absolute;
  margin-top: 209px;
  margin-left: 185px;
}

.rewardButton{
  position: absolute;
  margin-top: 265px;
  margin-left: 1px;
}
/*---------------------------------------------------*/
.ethButtonBlue{
  position: absolute;
  margin-top: 141px;
  margin-right: 74px;
  display: none;
}

.krkButtonBlue{
  position: absolute;
  margin-top: 142px;
  margin-left: 75px;
  display: none;
}

.ethWalletButtonBlue{
  position: absolute;
  margin-top: 205px;
  margin-right: 184px;
  display: none;
}

.krkWalletButtonBlue{
  position: absolute;
  margin-top: 209px;
  margin-left: 185px;
  display: none;
}

.rewardButtonBlue{
  position: absolute;
  margin-top: 265px;
  margin-left: 1px;
  display: none;
}
/*---------------------------------------------------*/

.ethField{
  position: absolute;
  width: 59px;
  height: 59px;
  z-index: 10;
  margin-top: 142px;
  margin-right: 73px;
  border-radius: 100px;
  cursor: pointer;
}

.krkField{
  position: absolute;
  width: 59px;
  height: 59px;
  z-index: 10;
  margin-top: 142px;
  margin-left: 75px;
  border-radius: 100px;
  cursor: pointer;
}

.ethWalletField{
  position: absolute;
  width: 59px;
  height: 59px;
  z-index: 10;
  border-radius: 100px;
  margin-top: 205px;
  margin-right: 184px;
  cursor: pointer;
}

.krkWalletField{
  position: absolute;
  width: 59px;
  height: 59px;
  z-index: 10;
  border-radius: 100px;
  margin-top: 209px;
  margin-left: 185px;
  cursor: pointer;
}

.rewardField{
  position: absolute;
  width: 59px;
  height: 59px;
  z-index: 10;
  border-radius: 100px;
  margin-top: 266px;
  margin-left: 1px;
  cursor: pointer;
}



.sendButton {
  width: 60px;
  position: absolute;
  margin-bottom: 32px;
  margin-left: 315px;
  background-color:#44c767;
  border-radius:28px;
  border:1px solid #18ab29;
  display:inline-block;
  cursor:pointer;
  color:#ffffff;
  font-family:Arial;
  font-size:14px;
  font-weight:bold;
  padding:2px 10px;
  text-decoration:none;
  text-shadow:0px 1px 0px #2f6627;
}
.sendButton:hover {
  background-color:#5cbf2a;
}
.sendButton:active {
  position:relative;
  top:1px;
}

.resetButton {
  position: absolute;
  margin-bottom: 32px;
  margin-right: 315px;
  background-color:#7892c2;
  border-radius:28px;
  border:1px solid #4e6096;
  display:inline-block;
  cursor:pointer;
  color:#ffffff;
  font-family:Arial;
  font-size:14px;
  font-weight:bold;
  padding:2px 10px;
  text-decoration:none;
  text-shadow:0px 1px 0px #283966;
}
.resetButton:hover {
  background-color:#476e9e;
}
.resetButton:active {
  position:relative;
  top:1px;
}
.textField{
  position: absolute;
  margin-bottom: 32px;
}

/*----------------BUTTONS END-----------------------------------*/





/*---------------------------------------------------*/
</style>
