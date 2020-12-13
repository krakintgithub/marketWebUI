<template>


  <div class="hasmetamask">
    <div class="popup" id="popup">
      <div class="popup-inner">


        <div class="sale">
          <img src="./assets/img/interface_final_open.png" alt="" width="100%" class="sale">
        </div>
      </div>

      <!--        <div class="install-metamask">Please install:<br><img src="./assets/img/metamask_logo.png" height="50px"><br>-->
      <!--          Metamask.<br> Then, reload...</div>-->

      <div class="refreshButton" v-on:click="reloadSale()"><img src="./assets/img/reload.png" width="32px"> </div>



      <!--        <div class="metamask-link"><a href="https://metamask.io/download.html" target="_blank">Click Here</a></div>-->
      <div class="balance">
        <div id="balance_num" style="display: inline-block;">Connect to Metamask...<br>Then, press reload.</div>
      </div>
      <div class="txtform"><input type="text" id="tbox" name="tbox" placeholder="Enter amount"></div>
      <div class="arrows"><img src="./assets/img/arrows.png" width="40px"></div>
      <div class="blankScreen"><img src="./assets/img/interface_final_closed.png" width="440px"></div>

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
      <div class="ethWalletField"> </div>
      <div class="krkWalletField"> </div>
      <div class="rewardField"> </div>


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
      userAddress: 'Connect to Metamask, then reload.',
      miners: '-1',
      totalBurned: '-1',
      totalMinted: '-1',
      blockNumber: '-1',
      reward: 'Connect to Metamask',
      deposit: '',
      withdraw: '',
    };
  },
  beforeMount() {
    this.refreshData();
  },
  methods: {
    ethButtonBlueShow(){
      if(!this.ethButtonClicked) document.getElementsByClassName('ethButtonBlue')[0].style.display = "block";
      },
    ethButtonBlueHide(){
      document.getElementsByClassName('ethButtonBlue')[0].style.display = "none";
      },
    ethButtonBlueClick(){
      this.resetButtons();
      this.ethButtonClicked = true;
      this.grayOutButtonsExcept('ethButton');
      document.getElementsByClassName('ethButtonBlue')[0].style.display = "none";
    },
    krkButtonBlueShow(){
      if(!this.krkButtonClicked) document.getElementsByClassName('krkButtonBlue')[0].style.display = "block";
    },
    krkButtonBlueHide(){
      document.getElementsByClassName('krkButtonBlue')[0].style.display = "none";
    },
    krkButtonBlueClick(){
      this.resetButtons();
      this.krkButtonClicked = true;
      this.grayOutButtonsExcept('krkButton');
      document.getElementsByClassName('krkButtonBlue')[0].style.display = "none";
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
    reloadSale(){window.location.reload(true);},
    refreshData() {
      this.userAddress = formatAddress(web3.eth.accounts.givenProvider.selectedAddress);
      const fromAddress = web3.eth.accounts.givenProvider.selectedAddress;

      auctionBox.methods
          .getCurrentBlockNumber()
          .call()
          .then((n) => {
            this.blockNumber = n;
            document.getElementById("block_num").innerText = this.blockNumber;
          });
      auctionBox.methods
          .showReward(fromAddress)
          .call()
          .then((n) => {
            this.reward = numberWithCommas(truncateNumber(web3.utils.fromWei(n, 'ether')));
            document.getElementById("balance_num").innerText = this.reward;
          });
    },

    processDeposit() {
      const fromAddress = web3.eth.accounts.givenProvider.selectedAddress;
      const tboxval = document.getElementById('tbox').value;
      const amount = web3.utils.toWei(formatNumber(tboxval), 'ether');
      this.deposit = '';

      auctionBox.methods
          .mine(amount).send({
        from: fromAddress,
      }).then(() => {
        document.getElementById('tbox').value = '';
      });
    },
    processWithdraw() {
      const fromAddress = web3.eth.accounts.givenProvider.selectedAddress;
      const tboxval = document.getElementById('tbox').value;
      const amount = web3.utils.toWei(formatNumber(tboxval), 'ether');
      this.withdraw = '';

      auctionBox.methods
          .getReward(amount).send({
        from: fromAddress,
      }).then(() => {
        document.getElementById('tbox').value = '';
      });

    }

  },
};
</script>

<style>
body {background:none transparent !important;
}

#tbox{
  width: 160px !important;
}


/*---------------------------------------------------*/
.arrows{
  position: absolute;
  margin-top: 142px;
  margin-right: -1px;
  z-index: 2;
}
.blankScreen{
  position: absolute;
}

/*---------------------------------------------------*/

.refreshButton {
  position: absolute;
  margin-top: -313px;
  margin-left: 339px;
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
  margin-top: 266px;
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
  margin-top: 266px;
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
  border: 1px solid #fff;
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
  border: 1px solid #fff;
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
  border: 1px solid #fff;
  position: absolute;
  width: 59px;
  height: 59px;
  z-index: 10;
  border-radius: 100px;
  margin-top: 266px;
  margin-left: 1px;
  cursor: pointer;
}
/*---------------------------------------------------*/





/*---------------------------------------------------*/
</style>
