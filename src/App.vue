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
        <div class="bold">Connected to:</div> {{this.userAddress.substr(0, 5) + "..." + this.userAddress.substr(35)}}<br><br>
        Click on a button<div><img src="./assets/img/hexagon_blank.png" width="50px"></div> from the menu below.
      </div>

      <div class="balance" v-if="!this.userAddress.startsWith('0x')">
        <div id="balance_num" style="display: inline-block;">Connect to Metamask...<br>Then, press reload.</div>
      </div>

     <div class="ethPurchase" style="display: none;"><img src="./assets/img/krk2eth.png" width="80px"><br>KRK to ETH</div>
     <div class="krkPurchase" style="display: none;"><img src="./assets/img/eth2krk.png" width="80px"><br>ETH to KRK</div>
     <div class="ethStats" style="display: none;"><img src="./assets/img/ethStats.png" width="80px"><br>ETH Statistics</div>
     <div class="krkStats" style="display: none;"><img src="./assets/img/krkStats.png" width="80px"><br>KRK Statistics</div>
     <div class="rewardStats" style="display: none;"><img src="./assets/img/reward.png" width="80px"><br>Earnings and Rewards</div>
     <div class="ethStats" style="display: none;">Displays<br><img src="./assets/img/ethStats.png" width="50px"><br>Ethereum statistics</div>


    <div class="ethPurchase_select" style="display: none;">
      <br>
      <div class="bold">Your ETH:</div> {{this.userEth}} ETH<br>
      <div class="bold">Your Fees:</div> {{this.totalUserFees}} ETH<br>
      <br>
      <div class="bold">Circulating:</div> {{this.circulatingEth}} ETH<br>
      <div class="bold">Deposited:</div> {{this.totalDepositedEth}} ETH<br>
      <div class="bold">Withdrawn:</div> {{this.totalWithdrawnEth}} ETH<br>
      <div class="bold">Fees:</div> {{this.totalFeesPaid}} ETH<br>
    </div>





      <div class="txtform"><input type="text" id="tbox" name="tbox" placeholder="Enter amount"></div>
      <div class="sendButton" style="display: none"><img src="./assets/img/send.png"></div>
      <div class="resetButton" style="display: none" v-on:click="clearTextField"><img src="./assets/img/reset.png"></div>
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
import auctionBox from '../contracts/auctionBoxInstance';






function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}

function truncateNumber(x) {
  return Math.trunc(x * 1000) / 1000;
}
//
// function formatNumber(x) {
//   return x.replace(/,/g, '');
// }

// function formatAddress(address) {
//   return address.substr(0, 5) + "..." + address.substr(35);
// }


export default {
  name: 'APP',
  created: function () {},
  data: function () {
    return {
      ethButtonClicked:false,
      krkButtonClicked:false,
      ethWalletButtonClicked:false,
      krkWalletButtonClicked:false,
      rewardButtonClicked:false,


      userAddress: 'Connect to Metamask',
      ethReturnNoBonus:0,
      ethReturnBonus:0,
      circulatingUserKrk:0,
      krkReturn:0,
      userEth:0,
      circulatingKrk:0,
      krakintTotalEthEarnings:0,
      totalBurnedKRK:0,
      totalMintedKRK:0,
      circulatingEth:0,
      totalUserFees:0,
      totalDepositedEth:0,
      totalWithdrawnEth:0,
      totalFeesPaid:0,
    };
  },
  beforeMount() {
    const addrr = web3.eth.accounts.givenProvider.selectedAddress;
    if(addrr!=null) {
      this.userAddress = addrr;
    }
   },
  methods: {
// ----------------BUTTONS START---------------
    ethButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethButtonClicked) return;
       document.getElementsByClassName('ethButtonBlue')[0].style.display = "block";
       document.getElementsByClassName('krkPurchase')[0].style.display = "block";
       document.getElementsByClassName('initMessage')[0].style.display = "none";
       document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";
      },
    ethButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethButtonClicked) return;
      document.getElementsByClassName('ethButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('krkPurchase')[0].style.display = "none";
        if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
          document.getElementsByClassName('initMessage')[0].style.display = "block";
        }
        if(this.ethWalletButtonClicked){ document.getElementsByClassName('ethPurchase_select')[0].style.display = "block";}
      },
    ethButtonBlueClick(){
      this.ethButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.ethButtonClicked = true;
      this.grayOutButtonsExcept('ethButton');
      this.enableTextField();
      document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";

    },
 //------------------
    krkButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkButtonClicked) return;
       document.getElementsByClassName('krkButtonBlue')[0].style.display = "block";
       document.getElementsByClassName('ethPurchase')[0].style.display = "block";
       document.getElementsByClassName('initMessage')[0].style.display = "none";
       document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";
    },
    krkButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkButtonClicked) return;
      document.getElementsByClassName('krkButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('ethPurchase')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      if(this.ethWalletButtonClicked){ document.getElementsByClassName('ethPurchase_select')[0].style.display = "block";}

    },
    krkButtonBlueClick(){
      this.krkButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.krkButtonClicked = true;
      this.grayOutButtonsExcept('krkButton');
      this.enableTextField();
      document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";

    },
    //------------------

    ethWalletButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethWalletButtonClicked) return;
       document.getElementsByClassName('ethWalletButtonBlue')[0].style.display = "block";
       document.getElementsByClassName('ethStats')[0].style.display = "block";
       document.getElementsByClassName('initMessage')[0].style.display = "none";
       document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";
    },
    ethWalletButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethWalletButtonClicked) return;
      document.getElementsByClassName('ethWalletButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('ethStats')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
        if(this.ethWalletButtonClicked){ document.getElementsByClassName('ethPurchase_select')[0].style.display = "block";}

      }
    },
    ethWalletButtonBlueClick(){
      this.ethWalletButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.ethWalletButtonClicked = true;
      this.grayOutButtonsExcept('ethWalletButton');
      this.disableTextField();
      this.getEthStats();
      document.getElementsByClassName('ethPurchase_select')[0].style.display = "block";
    },
    //------------------

    krkWalletButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkWalletButtonClicked) return;
       document.getElementsByClassName('krkWalletButtonBlue')[0].style.display = "block";
       document.getElementsByClassName('krkStats')[0].style.display = "block";
       document.getElementsByClassName('initMessage')[0].style.display = "none";
       document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";
    },
    krkWalletButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkWalletButtonClicked) return;
      document.getElementsByClassName('krkWalletButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('krkStats')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      if(this.ethWalletButtonClicked){ document.getElementsByClassName('ethPurchase_select')[0].style.display = "block";}

    },
    krkWalletButtonBlueClick(){
      this.krkWalletButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.krkWalletButtonClicked = true;
      this.grayOutButtonsExcept('krkWalletButton');
      this.disableTextField();
      this.getKrkStats();
      document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";
    },
    //------------------

    rewardButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.rewardButtonClicked) return;
       document.getElementsByClassName('rewardButtonBlue')[0].style.display = "block";
       document.getElementsByClassName('rewardStats')[0].style.display = "block";
       document.getElementsByClassName('initMessage')[0].style.display = "none";
       document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";
    },
    rewardButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.rewardButtonClicked) return;
       document.getElementsByClassName('rewardButtonBlue')[0].style.display = "none";
       document.getElementsByClassName('rewardStats')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      if(this.ethWalletButtonClicked){ document.getElementsByClassName('ethPurchase_select')[0].style.display = "block";}

    },
    rewardButtonBlueClick(){
      this.rewardButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.rewardButtonClicked = true;
      this.grayOutButtonsExcept('rewardButton');
      this.disableTextField();
      this.getRewardStats();
      document.getElementsByClassName('ethPurchase_select')[0].style.display = "none";

    },
    //------------------


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
    clearTextField(){
      document.getElementById('tbox').value = "";
    },
// ----------------CONTRACT FUNCTIONS START---------------
    getEthStats(){
      this.getKrakintTotalEthEarnings();
      this.getCirculatingEth();
      this.getTotalUserFees();
      this.getTotalDepositedEth();
      this.getTotalWithdrawnEth();
      this.getTotalFeesPaid();
    },
    getKrkStats(){
      this.getKrkReturn(web3.utils.toWei('1', 'ether'));
      this.getCirculatingKrk();
      this.getCirculatingUserKrk();
      this.getTotalBurnedKRK();
      this.getTotalMintedKRK();
    },
    getRewardStats(){
      this.getCirculatingUserKrk();
      this.getKrkReturn(this.circulatingUserKrk);
      this.getEthReturnBonus(this.circulatingUserKrk);
      this.getEthReturnNoBonus(this.circulatingUserKrk);
      this.getTotalUserFees(this.userAddress);
    },

    getEthReturnNoBonus(krkAmount){
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      auctionBox.methods
          .getEthReturnNoBonus(krkAmount, address)
          .call()
          .then((n) => {
            this.ethReturnNoBonus = n;
          });
    },
    getEthReturnBonus(krkAmount){
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      auctionBox.methods
          .getEthReturnBonus(krkAmount, address)
          .call()
          .then((n) => {
            this.ethReturnNoBonus = n;
          });
    },
    getKrkReturn(amount){
      auctionBox.methods
          .getKrkReturn(amount)
          .call()
          .then((n) => {
            this.krkReturn = n;
          });
    },
    getCirculatingKrk(){
      auctionBox.methods
          .getCirculatingKrk()
          .call()
          .then((n) => {
            this.circulatingKrk = n;
          });
    },
    getCirculatingEth(){
      auctionBox.methods
          .getCirculatingEth()
          .call()
          .then((n) => {
            this.circulatingEth = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getKrakintTotalEthEarnings(){
      auctionBox.methods
          .getKrakintTotalEthEarnings()
          .call()
          .then((n) => {
            this.krakintTotalEthEarnings = n;
          });
    },
    getUserEth(){
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      auctionBox.methods
          .getUserEth(address)
          .call()
          .then((n) => {
            this.userEth = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getCirculatingUserKrk(){
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      auctionBox.methods
          .getCirculatingUserKrk(address)
          .call()
          .then((n) => {
            this.circulatingUserKrk = n;
          });
    },
    getTotalUserFees(){
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      auctionBox.methods
          .getTotalUserFees(address)
          .call()
          .then((n) => {
            this.totalUserFees = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getTotalBurnedKRK(){
      auctionBox.methods
          .getTotalBurnedKRK()
          .call()
          .then((n) => {
            this.totalBurnedKRK = n;
          });
    },
    getTotalMintedKRK(){
      auctionBox.methods
          .getTotalMintedKRK()
          .call()
          .then((n) => {
            this.totalMintedKRK = n;
          });
    },
    getTotalDepositedEth(){
      auctionBox.methods
          .getTotalDepositedEth()
          .call()
          .then((n) => {
            this.totalDepositedEth = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getTotalWithdrawnEth(){
      auctionBox.methods
          .getTotalWithdrawnEth()
          .call()
          .then((n) => {
            this.totalWithdrawnEth = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getTotalFeesPaid(){
      auctionBox.methods
          .getTotalFeesPaid()
          .call()
          .then((n) => {
            this.totalFeesPaid = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
// ----------------CONTRACT FUNCTIONS END---------------

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
  position: absolute;
  margin-bottom: 32px;
  margin-left: 315px;
  cursor: pointer;
}

.resetButton {
  position: absolute;
  margin-bottom: 32px;
  margin-right: 315px;
  cursor: pointer;
}

.textField{
  display: block;
  position: absolute;
  margin-bottom: 30px;
}

/*----------------BUTTONS END-----------------------------------*/

/*----------------MENU START-----------------------------------*/

.ethPurchase{
  position: absolute;
  text-align: center;
  margin-bottom: 195px;
}
.krkPurchase{
  position: absolute;
  text-align: center;
  margin-bottom: 195px;
}
.ethStats{
  position: absolute;
  text-align: center;
  margin-bottom: 195px;
}
.krkStats{
  position: absolute;
  text-align: center;
  margin-bottom: 195px;
}
.rewardStats{
  position: absolute;
  text-align: center;
  margin-bottom: 195px;
}

.ethPurchase_select{
  position: absolute;
  text-align: left;
  margin-bottom: 195px;
}
/*----------------MENU END-----------------------------------*/


.bold{
  font-weight: bold;
  display:inline;
}

/*---------------------------------------------------*/
</style>
