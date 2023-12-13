<template>
  <div>
     <h1>Contact Book</h1>
     <ul>
       <li v-for="contact in sortedContacts" :key="contact.email" @click="showContactDetails($event, contact)">
         {{ contact.firstName }} {{ contact.lastName }}
       </li>
     </ul>

     <div v-if="selectedContact">
       <h2>Contact Details</h2>
       <p><strong>Name:</strong> {{ selectedContact.firstName }} {{ selectedContact.lastName }}</p>
       <p><strong>Email:</strong> {{ selectedContact.email }}</p>
       <button @click="editContact(contact)">Edit</button>
       <button @click="deleteContact(selectedContact)">Delete</button>
     </div>

 </div>

 <div>
    <form v-if="showEditForm" @submit.prevent="updateContact">
      <h2>Edit Contact</h2>
      <label for="editFirstName">First Name:</label>
      <input type="text" id="editFirstName" v-model="selectedContact.firstName" required>

      <label for="editLastName">Last Name:</label>
      <input type="text" id="editLastName" v-model="selectedContact.lastName" required>

      <label for="editEmail">Email:</label>
      <input type="email" id="editEmail" v-model="selectedContact.email" required>

      <button type="submit">Update</button>
    </form>

 </div>

  

  <div>
    <form @submit.prevent="addContact">
      <h2>Add New Contact</h2>
      <label for="firstName">First Name:</label>
      <input type="text" id="firstName" v-model="newContact.firstName" required>

      <label for="lastName">Last Name:</label>
      <input type="text" id="lastName" v-model="newContact.lastName" required>

      <label for="email">Email:</label>
      <input type="email" id="email" v-model="newContact.email" required>

      <button type="submit">Add Contact</button>
    </form>

  </div>
 
   

</template>



<script>

export default{
 data : function(){
   return{
    contactsList:[ 
     {firstName: "John", lastName: "Doe", email: "john@gmail.com "},
     {firstName: "Jane", lastName: "Smith", email: "jane@gmail.com "},
     {firstName: "Alice", lastName: "Johnson", email: "alice@gmail.com "},
     {firstName: "Bob", lastName: "Brown", email: "bob@gmail.com "},
     {firstName: "Emily", lastName: "Davis", email: "emily@gmail.com "},
     {firstName: "Michael", lastName: "Wilson", email: "michael@gmail.com "},
     {firstName: "Sophia", lastName: "Anderson", email: "sophia@gmail.com "},
     {firstName: "William", lastName: "Thomas", email: "emily@gmail.com "},
     {firstName: "Zoey", lastName: "Garcia", email: "emily@gmail.com "},
     {firstName: "James", lastName: "Anderson", email: "emily@gmail.com "},
     
   ],
   newContact: {
       firstName: "",
       lastName: "",
       email: "",
     },
   selectedContact: null,
   showEditForm: false
   }
 }, 

 computed: {
  sortedContacts() {
      return this.contactsList.slice().sort((a, b) => {
        return a.lastName.localeCompare(b.lastName)
      })
    },
   
 },

 methods: {
    showContactDetails(event,contact) {
      this.selectedContact = contact
      this.showEditForm = false

      const prevSelected = document.querySelector('li.selected')
      if (prevSelected) {
        prevSelected.classList.remove('selected')
      }

      const selectedElement = event.target
      selectedElement.classList.add('selected')
    },
    
    editContact(contact) {
      this.selectedContact = { ...contact }
      this.showEditForm = true
    },
  
    updateContact() {

      const index = this.contactsList.findIndex(
        (contact) => contact.email === this.selectedContact.email
      )

      if (index !== -1) {
        this.contactsList.splice(index, 1, this.selectedContact)
        
        localStorage.setItem("contacts", JSON.stringify(this.contactsList))

        this.selectedContact = null
        this.showEditForm = false
      }
    },

    addContact() {
      this.contactsList.push({
        firstName: this.newContact.firstName,
        lastName: this.newContact.lastName,
        email: this.newContact.email,
      })

      localStorage.setItem("contacts", JSON.stringify(this.contactsList))

      this.newContact = {
        firstName: "",
        lastName: "",
        email: "",
      }
    },

    deleteContact(contact) {
    const index = this.contactsList.findIndex(c => c.email === contact.email)

      if (index !== -1) {
        this.contactsList.splice(index, 1)
        localStorage.setItem("contacts", JSON.stringify(this.contactsList))
        this.selectedContact = null
        this.showEditForm = false
      }
    }
  
  
  }



}


</script>

     

<style>
    
  ul {
  list-style: none;
  padding: 0;
}

li {
  cursor: pointer;
  transition: all 0.3s ease; 
}

li.selected {
  font-weight: bold;
  background-color: aqua;
}

</style>
 