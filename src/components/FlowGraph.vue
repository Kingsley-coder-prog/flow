<template>
    <div>
      <div class="nav">
        <button id="triggers" @click="loadTriggers" :class="{'navactive': activeNav === 'triggers'}">Triggers</button>
        <button id="actions" @click="loadActions" :class="{'navactive': activeNav === 'actions'}">Actions</button>
      </div>
  
      <div id="blocklist" v-html="blockList"></div>
      <div id="canvas"></div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        activeNav: 'triggers',
        blockList: '',
      };
    },
    mounted() {
      this.loadTriggers();
      this.initializeFlowy();
    },
    methods: {
      initializeFlowy() {
        // Initialize Flowy and set drag, release, snapping handlers
        flowy(document.getElementById("canvas"), this.drag, this.release, this.snapping);
      },
      loadTriggers() {
        this.activeNav = 'triggers';
        this.blockList = `
          <div class="blockelem create-flowy noselect">
            <input type="hidden" name="blockelemtype" class="blockelemtype" value="1">
            <div class="grabme"><img src="assets/grabme.svg"></div>
            <div class="blockin">
              <div class="blockico"><span></span><img src="assets/eye.svg"></div>
              <div class="blocktext">
                <p class="blocktitle">New visitor</p>
                <p class="blockdesc">Triggers when somebody visits a specified page</p>
              </div>
            </div>
          </div>
          <div class="blockelem create-flowy noselect">
            <input type="hidden" name="blockelemtype" class="blockelemtype" value="2">
            <div class="grabme"><img src="assets/grabme.svg"></div>
            <div class="blockin">
              <div class="blockico"><span></span><img src="assets/action.svg"></div>
              <div class="blocktext">
                <p class="blocktitle">Action is performed</p>
                <p class="blockdesc">Triggers when somebody performs a specified action</p>
              </div>
            </div>
          </div>`;
      },
      loadActions() {
        this.activeNav = 'actions';
        this.blockList = `
          <div class="blockelem create-flowy noselect">
            <input type="hidden" name="blockelemtype" class="blockelemtype" value="5">
            <div class="grabme"><img src="assets/grabme.svg"></div>
            <div class="blockin">
              <div class="blockico"><span></span><img src="assets/database.svg"></div>
              <div class="blocktext">
                <p class="blocktitle">New database entry</p>
                <p class="blockdesc">Adds a new entry to a specified database</p>
              </div>
            </div>
          </div>
          <div class="blockelem create-flowy noselect">
            <input type="hidden" name="blockelemtype" class="blockelemtype" value="6">
            <div class="grabme"><img src="assets/grabme.svg"></div>
            <div class="blockin">
              <div class="blockico"><span></span><img src="assets/database.svg"></div>
              <div class="blocktext">
                <p class="blocktitle">Update database</p>
                <p class="blockdesc">Updates a specified database entry</p>
              </div>
            </div>
          </div>`;
      },
      drag(block) {
        block.classList.add("blockdisabled");
      },
      release() {
        const block = document.querySelector(".blockdisabled");
        if (block) {
          block.classList.remove("blockdisabled");
        }
      },
      snapping(drag, first) {
        const grab = drag.querySelector(".grabme");
        grab.parentNode.removeChild(grab);
        const blockin = drag.querySelector(".blockin");
        blockin.parentNode.removeChild(blockin);
  
        const blockType = drag.querySelector(".blockelemtype").value;
        let blockContent = '';
        
        switch (blockType) {
          case "1":
            blockContent = `
              <div class='blockyleft'>
                <img src='assets/eyeblue.svg'>
                <p class='blockyname'>New visitor</p>
              </div>
              <div class='blockyright'>
                <img src='assets/more.svg'>
              </div>
              <div class='blockydiv'></div>
              <div class='blockyinfo'>When a <span>new visitor</span> goes to <span>Site 1</span></div>`;
            break;
          case "2":
            blockContent = `
              <div class='blockyleft'>
                <img src='assets/actionblue.svg'>
                <p class='blockyname'>Action is performed</p>
              </div>
              <div class='blockyright'>
                <img src='assets/more.svg'>
              </div>
              <div class='blockydiv'></div>
              <div class='blockyinfo'>When <span>Action 1</span> is performed</div>`;
            break;
          case "5":
            blockContent = `
              <div class='blockyleft'>
                <img src='assets/databaseorange.svg'>
                <p class='blockyname'>New database entry</p>
              </div>
              <div class='blockyright'>
                <img src='assets/more.svg'>
              </div>
              <div class='blockydiv'></div>
              <div class='blockyinfo'>Add <span>Data object</span> to <span>Database 1</span></div>`;
            break;
          case "6":
            blockContent = `
              <div class='blockyleft'>
                <img src='assets/databaseorange.svg'>
                <p class='blockyname'>Update database</p>
              </div>
              <div class='blockyright'>
                <img src='assets/more.svg'>
              </div>
              <div class='blockydiv'></div>
              <div class='blockyinfo'>Update <span>Database 1</span></div>`;
            break;
          // Add more cases as needed...
        }
  
        drag.innerHTML += blockContent;
        return true;
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  