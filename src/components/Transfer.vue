<template>
  <div id="transfer" v-bind:class="{opened: showTransferPanel}">
    <div v-if="!transferCompleted">
      <h3>New transfer</h3>

      <form class="form" v-on:submit.prevent="onSubmit" method="post"  ref="transfer_form">
        <!-- Amount block -->
        <div class="form__group form__amount">
          <label for="amount">Amount</label>
          <input id="amount" name="transfer_amount"
            type="number"step=".01"
            @input="checkTransferAmount">
          <p>{{account.coin.toUpperCase()}}</p>
        </div>

        <!-- Address block -->
        <div
        v-bind:class="[isValidAddress && 'address--valid', isValidAddress === false && 'address--error', 'form__group form__address']">
          <label for="address">Address</label>
          <input type="text" id="address" name="transfer_address" @input="checkTransferAddress">
          <span class="valid__icon" v-if="isValidAddress">
            <i class="material-icons">
              check_circle_outline
            </i>
            <span>The address is valid</span>
          </span>
          <span class="valid__icon" v-if="isValidAddress === false">
            <i class="material-icons">
              error_outline
            </i>
            <span>The address is not valid</span>
          </span>
          <button class="btn btn--small btn--yellow contacts__show" type="button" name="button"
            v-on:click="showContactsPanel = !showContactsPanel">
            View Contacts
            <i class="material-icons">
              {{showContactsPanel ? 'expand_less' : 'expand_more'}}
            </i>
          </button>
        </div>

        <!-- Submit block -->
        <div class="form__group for__submit">
          <button class="btn btn--big" type="submit"
            :disabled="!isValidAddress || !isValidAmount">
            Transfer
          </button>
        </div>
      </form>
      <!-- Contacts agenda -->
      <Agenda
        v-if="showContactsPanel"
        :onClick="selectContact"
        :contacts="contacts"
      />
      <!-- Close panel -->
      <div class="panel__close" v-on:click="toggleTransferPanel()">
        <i class="material-icons">close</i>
      </div>
    </div>

    <!-- Completed transfer, hidden by default -->
    <div v-if="transferCompleted"  class="transfer__completed">
      <div v-bind:class="['completed__content', transferCompleted && 'active']">
        Tranfer completed succesfully!
        <i class="material-icons">
          check_circle_outline
        </i>
      </div>
    </div>
  </div>
</template>

<script>
  import Agenda from "./Agenda.vue"

  export default {
    name: 'transfer',
    data() {
      return {
        search: '',
        showContactsPanel: false,
        isValidAddress: null,
        isValidAmount: false,
        transferCompleted: false,
      }
    },
    props: {
      contacts: {
        type: Array,
        default: () => []
      },
      showTransferPanel: {
        type: Boolean,
        default: () => false
      },
      onCompleted: {
        type: Function,
        default: () => null
      },
      account: {
        type: Object,
        default: () => {}
      },
      toggleTransferPanel: {
        type: Function,
        default: () => null
      }
    },
    methods: {
      // On submit form animate, close, clear and propagate to parent onSubmit
      onSubmit(e) {
        const amount = e.target.elements["amount"].value
        const address = e.target.elements["address"].value

        this.showContactsPanel && (this.showContactsPanel = false)
        this.transferSuccess()

        // Restore form view for new transfer
        this.onCompleted(amount, address, + new Date())
        this.clearForm(e.target)
      },
      // Animation on transfer completed
      transferSuccess() {
        const self = this;
        // Show success message after X ms
        this.transferCompleted = true
        // Close panel after X ms
        setTimeout(() => self.toggleTransferPanel(), 2500)
        // Restore transfer view
        setTimeout(() => this.transferCompleted = false, 3000)
      },
      // Clear the form fields
      clearForm(form) {
        form.elements["amount"].value = null
        form.elements["address"].value = ''
        this.isValidAddress = null
        this.isValidAmount = null
      },
      // Use contact address for transfer
      selectContact(contact) {
        const inputElement = this.$refs.transfer_form["address"]
        inputElement.value = contact.address
        this.checkTransferAddress()
        this.showContactsPanel = false
      },
      // Test if address is 51 chars length
      checkTransferAddress(e) {
        // This method can be called when editing the input or when selectContact (DOM insertion)
        const inputValue = e ? e.target.value : this.$refs.transfer_form["address"].value
        const isValid = inputValue !== '' ? inputValue.length === 51 : null

        this.isValidAddress = isValid
      },
      // Test if user have enough amount to do the transfer
      checkTransferAmount(e) {
        // Convert both values to numbers
        const inputValue = Number(e.target.value)
        const currentBalance = Number(this.account.balance)
        const isValid = inputValue > 0 && typeof inputValue == 'number' && inputValue <= currentBalance

        this.isValidAmount = isValid
      }
    },
    components: {
      Agenda,
    },
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/transfer.scss';
</style>
