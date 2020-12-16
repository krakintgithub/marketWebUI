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


    <div class="ethStats_select" style="display: none;">
      <br>
      <div class="bold">Your ETH:</div> {{this.userEth}} ETH<br>
      <div class="bold">Your Fees:</div> {{this.totalUserFees}} ETH<br>
      <br>
      <div class="bold">Circulating:</div> {{this.circulatingEth}} ETH<br>
      <div class="bold">Deposited:</div> {{this.totalDepositedEth}} ETH<br>
      <div class="bold">Withdrawn:</div> {{this.totalWithdrawnEth}} ETH<br>
      <div class="bold">Fees:</div> {{this.totalFeesPaid}} ETH<br>
    </div>

      <div class="krkStats_select" style="display: none;">
        <br>
        <div class="bold">Your KRK:</div> {{this.circulatingUserKrk}} KRK<br>
        <br>
        <div class="bold">Circulating:</div> {{this.circulatingKrk}} KRK<br>
        <div class="bold">Burned:</div> {{this.totalBurnedKRK}} KRK<br>
        <div class="bold">Minted:</div> {{this.totalMintedKRK}} KRK<br>
      </div>

      <div class="rewardStats_select" style="display: none;">
        <div class="bold">Your Reward:</div> {{this.ethReturnBonus}} ETH<br>
        <div class="bold">Without Reward:</div> {{this.ethReturnNoBonus}} ETH<br>
        <div class="bold">Your Total:</div> {{this.ethTotalReturn}} ETH<br>
        <br>
        <div class="bold">Shared Earnings:</div> {{this.krakintTotalEthEarnings}} ETH<br>
      </div>

      <div class="eth_select" style="display: none;">
        Exchange KRK for ETH<br><br>
        <div class="bold">Rate:</div> {{this.returnRate}} ETH for 1000 KRK<br>
        You can only exchange the amount you purchased.
      </div>

      <div class="krk_select" style="display: none;">
        Purchase KRK with ETH<br><br>
        <div class="bold">Rate:</div> {{this.currentRate}} KRK for 1 ETH<br>
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

function formatNumber(x) {
  return x.replace(/,/g, '');
}

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
      ethTotalReturn:0,
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
      currentRate:0,
      returnRate:0,
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
    switchOff(off){
      for (let i = 0; i < off.length; i++) {
        document.getElementsByClassName(off[i])[0].style.display = "none";
      }
    },
    switchOn(on){
      for (let i = 0; i < on.length; i++) {
        document.getElementsByClassName(on[i])[0].style.display = "block";
      }
    },
    blueHide(){
      if(this.ethWalletButtonClicked){ document.getElementsByClassName('ethStats_select')[0].style.display = "block";}
      else if(this.krkWalletButtonClicked){ document.getElementsByClassName('krkStats_select')[0].style.display = "block";}
      else if(this.rewardButtonClicked){ document.getElementsByClassName('rewardStats_select')[0].style.display = "block";}
      else if(this.ethButtonClicked || this.krkButtonClicked){
        document.getElementsByClassName('txtform')[0].style.display = "block";
        document.getElementsByClassName('sendButton')[0].style.display = "block";
        document.getElementsByClassName('resetButton')[0].style.display = "block";
        document.getElementsByClassName('textField')[0].style.display = "block";
        if(this.ethButtonClicked){document.getElementsByClassName('eth_select')[0].style.display = "block";}
        if(this.krkButtonClicked){document.getElementsByClassName('krk_select')[0].style.display = "block";}
      }
    },
    switchBlueShow(on){
      var off = ["ethButtonBlue","krkPurchase","initMessage","ethStats_select","krkStats_select","rewardStats_select",
      "txtform","sendButton","resetButton","textField","eth_select","krk_select"];
      this.switchOff(off);
      this.switchOn(on);
    },
    ethButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethButtonClicked) return;
      var on = ["ethButtonBlue","krkPurchase"];
      this.switchBlueShow(on);
      },
    ethButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethButtonClicked) return;
      document.getElementsByClassName('ethButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('krkPurchase')[0].style.display = "none";
        if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
          document.getElementsByClassName('initMessage')[0].style.display = "block";
        }
      this.blueHide();
      },
    ethButtonBlueClick(){
      this.ethButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.ethButtonClicked = true;
      this.grayOutButtonsExcept('ethButton');
      this.enableTextField();
      this.getReturnRate();

      var off = ["ethStats_select","krkStats_select","rewardStats_select","rewardStats_select","krk_select"];
      var on = ["txtform","sendButton","resetButton","textField","eth_select"];

      this.switchOff(off);
      this.switchOn(on);
    },
 //------------------
    krkButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkButtonClicked) return;
      var on = ["krkButtonBlue","ethPurchase"];
      this.switchBlueShow(on);
    },
    krkButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkButtonClicked) return;
      document.getElementsByClassName('krkButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('ethPurchase')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      this.blueHide();
    },
    krkButtonBlueClick(){
      this.krkButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.krkButtonClicked = true;
      this.grayOutButtonsExcept('krkButton');
      this.enableTextField();
      this.getCurrentRate();

      var off = ["ethStats_select","krkStats_select","rewardStats_select","eth_select"];
      var on = ["txtform","sendButton","resetButton","textField","krk_select"];

      this.switchOff(off);
      this.switchOn(on);
    },
    //------------------
    ethWalletButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethWalletButtonClicked) return;
      var on = ["ethWalletButtonBlue","ethStats"];
      this.switchBlueShow(on);
    },
    ethWalletButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.ethWalletButtonClicked) return;
      document.getElementsByClassName('ethWalletButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('ethStats')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      this.blueHide();
    },
    ethWalletButtonBlueClick(){
      this.ethWalletButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.ethWalletButtonClicked = true;
      this.grayOutButtonsExcept('ethWalletButton');
      this.disableTextField();
      this.getEthStats();

      var off = ["krkStats_select","rewardStats_select","txtform","sendButton","resetButton","textField","eth_select","krk_select"];
      var on = ["ethStats_select"];

      this.switchOff(off);
      this.switchOn(on);
    },
    //------------------
    krkWalletButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkWalletButtonClicked) return;
      var on = ["krkWalletButtonBlue","krkStats"];
      this.switchBlueShow(on);
    },
    krkWalletButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.krkWalletButtonClicked) return;
      document.getElementsByClassName('krkWalletButtonBlue')[0].style.display = "none";
      document.getElementsByClassName('krkStats')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      this.blueHide();
    },
    krkWalletButtonBlueClick(){
      this.krkWalletButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.krkWalletButtonClicked = true;
      this.grayOutButtonsExcept('krkWalletButton');
      this.disableTextField();
      this.getKrkStats();

      var off = ["ethStats_select","rewardStats_select","txtform","sendButton","resetButton","textField","eth_select","krk_select"];
      var on = ["krkStats_select"];

      this.switchOff(off);
      this.switchOn(on);
    },
    //------------------
    rewardButtonBlueShow(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.rewardButtonClicked) return;
      var on = ["rewardButtonBlue","rewardStats"];
      this.switchBlueShow(on);
    },
    rewardButtonBlueHide(){
      if(!this.userAddress.startsWith('0x')) return;
      if(this.rewardButtonClicked) return;
       document.getElementsByClassName('rewardButtonBlue')[0].style.display = "none";
       document.getElementsByClassName('rewardStats')[0].style.display = "none";
      if(!this.ethButtonClicked && !this.krkButtonClicked && !this.ethWalletButtonClicked && !this.krkWalletButtonClicked && !this.rewardButtonClicked) {
        document.getElementsByClassName('initMessage')[0].style.display = "block";
      }
      this.blueHide();
    },
    rewardButtonBlueClick(){
      this.rewardButtonBlueHide();
      if(!this.userAddress.startsWith('0x')) return;
      this.resetButtons();
      this.rewardButtonClicked = true;
      this.grayOutButtonsExcept('rewardButton');
      this.disableTextField();
      this.getRewardStats();

      var off = ["ethStats_select","krkStats_select","txtform","sendButton","resetButton","textField","eth_select","krk_select"];
      var on = ["rewardStats_select"];

      this.switchOff(off);
      this.switchOn(on);
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
      var on = ["ethButton","krkButton","ethWalletButton","krkWalletButton","rewardButton"];
      this.switchOn(on);
      document.getElementsByClassName(bttnExcpt)[0].style.display = "none";
    },

    disableTextField(){
      var off = ["ethWalletButtonBlue","initMessage","txtform","sendButton","resetButton","textField"];
      this.switchOff(off);
    },
    enableTextField(){
      var off = ["krkButtonBlue","initMessage"];
      var on = ["txtform","sendButton","resetButton","textField"];

      this.switchOff(off);
      this.switchOn(on);
    },
// ----------------BUTTONS END---------------
    reloadSale(){window.location.reload(true);},
    clearTextField(){
      document.getElementById('tbox').value = "";
    },
    send(){
      var tboxval = document.getElementById('tbox').value;
      const fromAddress = web3.eth.accounts.givenProvider.selectedAddress;
      const amountWei = web3.utils.toWei(formatNumber(tboxval), 'ether');
      if(this.ethButtonClicked){
        //alert("ethButtonClicked");
        console.log(123);
        auctionBox.methods
            .purchaseEthereum(amountWei)
            .send({ from: fromAddress });

      }
      else if(this.krkButtonClicked){
        //alert("krkButtonClicked");
        auctionBox.methods
            .purchaseTokens()
            .send({ from: fromAddress, value:amountWei });
      }
    },
// ----------------CONTRACT FUNCTIONS START---------------
    getCurrentRate(){
        auctionBox.methods
            .getKrkReturn( web3.utils.toWei('1', 'ether'))
            .call()
            .then((n) => {
              this.currentRate = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
            });
    },
    getReturnRate(){
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      let nobonus = 0;
      let bonus = 0;

      auctionBox.methods
          .getEthReturnBonus(web3.utils.toWei('1000', 'ether'), address)
          .call()
          .then((n) => {
            nobonus = n;
          });

      auctionBox.methods
          .getEthReturnNoBonus(web3.utils.toWei('1000', 'ether'), address)
          .call()
          .then((n) => {
            bonus = n;
          });

      let rate_n = web3.utils.fromWei(''+nobonus, 'ether');
      let rate_b = web3.utils.fromWei(''+bonus, 'ether');
      let rr = Number(rate_n)+Number(rate_b);
      this.returnRate = numberWithCommas(truncateNumber(rr));

    },
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

      var circKRK = 0;
      const address = web3.eth.accounts.givenProvider.selectedAddress;
      auctionBox.methods
          .getCirculatingUserKrk(address)
          .call()
          .then((n) => {
            circKRK = n;
          }).then(()=>{
        this.getKrkReturn(circKRK);
        this.getEthReturnBonus(circKRK);
        this.getEthReturnNoBonus(circKRK);
        this.getKrakintTotalEthEarnings(circKRK);
      });
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
            this.ethReturnNoBonus = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getKrkReturn(amount){
      auctionBox.methods
          .getKrkReturn(amount)
          .call()
          .then((n) => {
            this.krkReturn = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getCirculatingKrk(){
      auctionBox.methods
          .getCirculatingKrk()
          .call()
          .then((n) => {
            this.circulatingKrk = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
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
            this.krakintTotalEthEarnings = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
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
            this.circulatingUserKrk = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
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
            this.totalBurnedKRK = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
          });
    },
    getTotalMintedKRK(){
      auctionBox.methods
          .getTotalMintedKRK()
          .call()
          .then((n) => {
            this.totalMintedKRK = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
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
.eth_select, .krk_select, .rewardStats, .krkStats, .ethStats, .krkPurchase, .ethPurchase{
  position: absolute;
  text-align: center;
  margin-bottom: 195px;
}

.krkStats_select, .rewardStats_select, .ethStats_select{
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
