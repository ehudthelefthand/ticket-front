<template>
  <div id="app">
    <form>
      <div class="field">
        <div class="label">
          <label for="type">ประเภท :</label>
        </div>
        <div class="content">
          <input id="type" type="text" placeholder="ประเภท" autofocus v-model="ticket.type" />
        </div>
      </div>
      <div class="field">
        <div class="label">
          <label for="description">รายละเอียด :</label>
        </div>
        <div class="content">
          <input id="description" type="text" placeholder="รายละเอียด" v-model="ticket.description" />
        </div>
      </div>
      <div class="field">
        <div class="label">
          <label for="fine">ค่าปรับ (บาท) :</label>
        </div>
        <div class="content">
          <input id="fine" type="number" v-model="ticket.fine" />
        </div>
      </div>
      <div class="field">
        <div class="label">
          <span>สถานะการจ่ายเงิน :</span>
        </div>
        <div class="content">
          <div class="radio">
            <input id="isPaid-yes" type="radio" name="isPaid" v-model="ticket.isPaid" value="yes" />
            <label for="isPaid-yes">จ่ายแล้ว</label>
          </div>
          <div class="radio">
            <input id="isPaid-no" type="radio" name="isPaid" v-model="ticket.isPaid" value="no" />
            <label for="isPaid-no">ยังไม่ได้จ่าย</label>
          </div>
        </div>
      </div>
      <div class="field">
        <div clsas="content">
          <button v-if="ticket.id !== undefined" @click.prevent="updateTicket">บันทึก</button>
          <button v-else @click.prevent="addTicket">เพิ่ม</button>
          <button @click.prevent="resetForm">ยกเลิก</button>
        </div>
      </div>
    </form>
    <table>
      <thead>
        <tr>
          <th>ประเภท</th>
          <th>รายละเอียด</th>
          <th>ค่าปรับ (บาท)</th>
          <th>สถานะการจ่ายเงิน</th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="ticket in tickets" :key="ticket.id">
          <td>{{ ticket.type }}</td>
          <td>{{ ticket.description }}</td>
          <td>{{ ticket.fine }}</td>
          <td
            :class="{ yes: ticket.isPaid, no: !ticket.isPaid }"
          >{{ ticket.isPaid ? 'จ่ายแล้ว' : 'ยังไม่ได้จ่าย' }}</td>
          <td>
            <button class="small" @click="editTicket(ticket)">แก้ไข</button>
          </td>
          <td>
            <button class="small" @click="togglePaid(ticket)">ปรับสถานะการจ่ายเงิน</button>
          </td>
          <td>
            <button class="small" @click="deleteTikcet(ticket)">ลบ</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      ticket: {
        type: "",
        description: "",
        fine: 0,
        isPaid: "no"
      },
      tickets: [
        {
          id: 0,
          type: "something",
          description: "test",
          fine: 500,
          isPaid: false
        }
      ]
    };
  },
  methods: {
    addTicket() {
      if (
        this.ticket.type.trim() === "" ||
        this.ticket.description.trim() === ""
      ) {
        return;
      }
      const newTicket = {
        ...this.ticket,
        id: this.tickets.length,
        isPaid: this.ticket.isPaid === "yes" ? true : false
      };
      this.tickets = [...this.tickets, newTicket];
      this.resetForm();
    },
    resetForm() {
      this.ticket = {
        type: "",
        description: "",
        fine: 0,
        isPaid: "no"
      };
    },
    editTicket(ticket) {
      this.ticket = { ...ticket, isPaid: ticket.isPaid ? "yes" : "no" };
    },
    updateTicket() {
      if (
        this.ticket.type.trim() === "" ||
        this.ticket.description.trim() === ""
      ) {
        return;
      }
      const targetIndex = this.tickets.findIndex(t => t.id === this.ticket.id);
      if (targetIndex !== -1) {
        const newTicket = {
          ...this.ticket,
          isPaid: this.ticket.isPaid === "yes" ? true : false
        };
        this.tickets.splice(targetIndex, 1, newTicket);
      }
      this.resetForm();
    },
    deleteTikcet(ticket) {
      this.tickets = this.tickets.filter(t => t.id !== ticket.id);
    },
    togglePaid(ticket) {
      const targetIndex = this.tickets.findIndex(t => t.id === ticket.id);
      if (targetIndex !== -1) {
        const target = this.tickets[targetIndex];
        target.isPaid = !target.isPaid;
      }
    }
  }
};
</script>

<style scoped>
#app {
  width: 1000px;
  margin: 2rem auto;
  color: #222;
}
form {
  width: 500px;
  margin: 0 auto 4rem;
  font-size: 1rem;
}
.field {
  margin: 1rem;
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-items: center;
}
.field > .label {
  flex: 1;
  text-align: right;
  margin-right: 1rem;
  font-weight: 700;
}
.field > .content {
  flex: 2;
}
.field > .content input[type="text"],
.field > .content input[type="number"] {
  padding: 8px 16px;
  width: 100%;
  font-size: 1rem;
  border: 1px solid #333;
  border-radius: 5px;
  outline: none;
}
input[type="text"]::-webkit-input-placeholder,
input[type="number"]::-webkit-input-placeholder {
  font-family: "Kanit", sans-serif;
}
input[type="text"]:-ms-input-placeholder,
input[type="number"]:-ms-input-placeholder {
  font-family: "Kanit", sans-serif;
}
input[type="text"]:-moz-placeholder,
input[type="number"]:-moz-placeholder {
  font-family: "Kanit", sans-serif;
}
input[type="text"]::-moz-placeholder,
input[type="number"]::-moz-placeholder {
  font-family: "Kanit", sans-serif;
}
.field > .content .radio {
  display: inline-block;
  margin-right: 20px;
}
.field > .content .radio > input[type="radio"] {
  margin-right: 10px;
}
.field:last-child {
  margin-top: 2rem;
}

table {
  width: 100%;
  padding: 20px;
  border: 1px solid #333;
  table-layout: auto;
}
table th {
  font-weight: 700;
}
table th,
table td {
  border: 1px solid #333;
  padding: 20px;
}
table th:nth-child(1),
table th:nth-child(2),
table td:nth-child(1),
table td:nth-child(2) {
  text-align: left;
}

table th:nth-child(3),
table th:nth-child(4),
table td:nth-child(3),
table td:nth-child(4) {
  text-align: center;
}

table th:nth-child(5),
table th:nth-child(6),
table th:nth-child(7) {
  width: 1%;
}
table td.yes {
  color: green;
}
table td.no {
  color: red;
}

button {
  width: 150px;
  padding: 8px 16px;
  font-family: inherit;
  font-size: 1rem;
  background-color: #fff;
  border: 1px solid #333;
  border-radius: 5px;
  margin: 0 5px;
  outline: none;
  cursor: pointer;
}
button:hover {
  color: #fff;
  background-color: #333;
}
button:active {
  color: #333;
  background-color: #fff;
}
button.small {
  width: 100px;
  font-size: 0.8rem;
  padding: 4px 8px;
}
</style>
