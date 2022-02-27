<template>
  <div
    @click="checkClick"
    ref="invoiceRef"
    class="invoice-wrap flex flex-column"
  >
    <form @submit.prevent="submitInvoice" class="invoice-content">
      <h1>New Invoice</h1>
      <!-- Bill From Section -->
      <div class="bill-from flex flex-column">
        <h4>Bill From</h4>
        <div class="input flex flex-column">
          <label for="billerStreetAddress">Street Address</label>
          <input
            type="text"
            v-model="billerStreetAddress"
            id="billerStreetAddress"
            placeholder="Enter Street Address"
            required
          />
        </div>
        <div class="location-details flex">
          <div class="input flex flex-column">
            <label for="billerCity">City</label>
            <input
              type="text"
              v-model="billerCity"
              id="billerCity"
              placeholder="Enter City"
              required
            />
          </div>
          <div class="input flex flex-column">
            <label for="billerZipCode">Zip Code</label>
            <input
              type="text"
              v-model="billerZipCode"
              id="billerZipCode"
              placeholder="Enter Zip Code"
              required
            />
          </div>
          <div class="input flex flex-column">
            <label for="billerCountry">Country</label>
            <input
              type="text"
              v-model="billerCountry"
              id="billerCountry"
              placeholder="Enter Country"
              required
            />
          </div>
        </div>
      </div>

      <!-- Bill To Section -->
      <div class="bill-to flex flex-column">
        <h4>Bill to</h4>
        <div class="input flex flex-column">
          <label for="clientName">Client's Name</label>
          <input
            type="text"
            v-model="clientName"
            id="clientName"
            placeholder="Enter Name"
            required
          />
        </div>
        <div class="input flex flex-column">
          <label for="clientEmail">Client's Email</label>
          <input
            type="email"
            v-model="clientEmail"
            id="clientEmail"
            placeholder="Enter Email"
            required
          />
        </div>
        <div class="input flex flex-column">
          <label for="clientStreetAddress">Street Address</label>
          <input
            type="text"
            v-model="clientStreetAddress"
            id="clientStreetAddress"
            placeholder="Enter Street Address"
            required
          />
        </div>
        <div class="location-details flex">
          <div class="input flex flex-column">
            <label for="clientCity">City</label>
            <input
              type="text"
              v-model="clientCity"
              id="clientCity"
              placeholder="Enter City"
              required
            />
          </div>
          <div class="input flex flex-column">
            <label for="clientZipCode">Zip Code</label>
            <input
              type="text"
              v-model="clientZipCode"
              id="clientZipCode"
              placeholder="Enter Zip Code"
              required
            />
          </div>
          <div class="input flex flex-column">
            <label for="clientCountry">Country</label>
            <input
              type="text"
              v-model="clientCountry"
              id="clientCountry"
              placeholder="Enter Country"
              required
            />
          </div>
        </div>
      </div>

      <!-- Invoice Work Details -->
      <div class="invoice-work flex flex-column">
        <div class="payment flex">
          <div class="input flex flex-column">
            <label for="invoiceDate">Invoice Date</label>
            <input disabled v-model="invoiceDate" id="invoiceDate" />
          </div>
          <div class="input flex flex-column">
            <label for="paymentDueDate">Payemnt Due Date</label>
            <input disabled v-model="paymentDueDate" id="paymentDueDate" />
          </div>
        </div>
        <div class="input flex flex-column">
          <label for="paymentTerms">Payemnt Terms</label>
          <select v-model="paymentTerms" id="paymentTerms" required>
            <option value="30">Net 30 Days</option>
            <option value="60">Net 60 Days</option>
            <option value="90">Net 90 Days</option>
          </select>
        </div>
        <div class="input flex flex-column">
          <label for="productDescription">Payemnt Description</label>
          <input
            type="text"
            v-model="productDescription"
            id="productDescription"
            placeholder="Enter Description"
            required
          />
        </div>
        <div class="work-items">
          <h3>Item List</h3>
          <table class="item-list">
            <tr class="table-heading flex">
              <th class="item-name">Item Name</th>
              <th class="qty">Qty</th>
              <th class="price">Price</th>
              <th class="total">Total</th>
            </tr>
            <tr
              class="table-items flex"
              v-for="(item, index) in invoiceItemList"
              :key="index"
            >
              <td class="item-name">
                <input type="text" v-model="item.itemName" />
              </td>
              <td class="qty">
                <input type="number" v-model="item.qty" />
              </td>
              <td class="price">
                <input type="number" v-model="item.price" />
              </td>
              <td class="total flex">
                ${{ (item.total = item.price * item.qty) }}
              </td>
              <img
                @click="deleteInvoiceItem(item.id)"
                src="@/assets/icon-delete.svg"
                alt="item-delete"
              />
            </tr>
          </table>
          <div @click="addNewInvoiceItem" class="flex button">
            <img src="@/assets/icon-plus.svg" alt="item-add" />
            Add New Item
          </div>
        </div>
      </div>

      <!-- Save/Exit -->
      <div class="save flex">
        <div class="left">
          <button @click="closeInvoice" class="red">Cancel</button>
        </div>
        <div class="right flex">
          <button @click="saveDraft" class="dark-purple">Save Draft</button>
          <button @click="publishInvoice" class="purple">Create Invoice</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  name: "InvoiceModal",
  data() {
    return {
      billerStreetAddress: null,
      billerCity: null,
      billerZipCode: null,
      billerCountry: null,
      clientName: null,
      clientEmail: null,
      clientStreetAddress: null,
      clientCity: null,
      clientZipCode: null,
      clientCountry: null,
      invoiceDateUnix: null,
      invoiceDate: null,
      paymentTerms: null,
      paymentDueDateUnix: null,
      paymentDueDate: null,
      productDescription: null,
      invoicePending: null,
      invoiceDraft: null,
      invoiceItemList: [],
      invoiceTotal: 0,
    };
  },
  methods: {
    ...mapMutations(["TOGGLE_INVOICE"]),
    closeInvoice() {
      this.TOGGLE_INVOICE();
    },
  },
};
</script>

<style lang="scss" scoped>
.invoice-wrap {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  overflow: scroll;
  &::-webkit-scrollbar {
    display: none;
  }
  @media (min-width: 900px) {
    left: 90px;
  }

  .invoice-content {
    position: relative;
    padding: 55px;
    max-width: 700px;
    width: 100%;
    background-color: #141625;
    color: #fff;
    box-shadow: 10px 4px 6px -1px rgba(0, 0, 0, 0.2),
      0 2px 4px -1px rgba(0, 0, 0, 0.06);

    h1 {
      margin-bottom: 48px;
      color: #fff;
    }
    h3 {
      margin-bottom: 16px;
      font-size: 18px;
      color: #777f98;
    }
    h4 {
      color: #7c5dfa;
      font-size: 12px;
      margin-bottom: 24px;
    }

    //Bill to/Bill from
    .bill-to,
    .bill-from {
      margin-bottom: 40px;

      .location-details {
        gap: 16px;

        div {
          flex: 1;
        }
      }
    }

    //invoice work
    .invoice-work {
      .payment {
        gap: 24px;
        div {
          flex: 1;
        }
      }

      .work-items {
        .item-list {
          width: 100%;

          //Item table style
          .table-heading,
          .table-items {
            gap: 16px;
            font-size: 12px;

            .item-name {
              flex-basis: 50%;
            }

            .qty {
              flex-basis: 10%;
            }

            .price {
              flex-basis: 20%;
            }

            .total {
              flex-basis: 20%;
              align-self: center;
            }
          }

          .table-heading {
            margin-bottom: 16px;

            th {
              text-align: left;
            }
          }

          .table-items {
            position: relative;
            margin-bottom: 24px;

            img {
              position: absolute;
              top: 15px;
              right: 0;
              width: 12px;
              height: 16px;
            }
          }
        }

        .button {
          color: #fff;
          background-color: #252545;
          align-items: center;
          justify-content: center;
          width: 100%;

          img {
            margin-right: 4px;
          }
        }
      }
    }

    .save {
      margin-top: 60px;

      div {
        flex: 1;
      }

      .right {
        justify-content: flex-end;
      }
    }
  }

  .input {
    margin-bottom: 24px;
  }

  label {
    font-size: 12px;
    margin: 6px;
  }

  input,
  select {
    width: 100%;
    background-color: #1e2139;
    color: #fff;
    border-radius: 8px;
    padding: 10px 4px;
    border: none;

    &:focus {
      outline: none;
    }

    &::placeholder {
      color: rgb(194, 137, 137);
      opacity: 1;
      padding: 10px;
    }
  }
}
</style>
