<template>
    <div id="app" class="small-container">
        <h1>Employees</h1>

        <employee-form @add:employee="addEmployee"/>
        <employee-table
                :employees="employees"
                @delete:employee="deleteEmployee"
                @edit:employee="editEmployee"
        />
    </div>
</template>

<script>
    import EmployeeTable from '@/components/EmployeeTable.vue'
    import EmployeeForm from '@/components/EmployeeForm.vue'

    export default {
        name: 'app',
        components: {
            EmployeeTable,
            EmployeeForm,
        },
        data() {
            return {
                employees: [
                    {
                        id: 1,
                        name: 'Richard Hendricks',
                        email: 'richard@piedpiper.com',
                    },
                    {
                        id: 2,
                        name: 'Bertram Gilfoyle',
                        email: 'gilfoyle@piedpiper.com',
                    },
                    {
                        id: 3,
                        name: 'Dinesh Chugtai',
                        email: 'dinesh@piedpiper.com',
                    },
                ],
            }
        },
        methods: {
            async getEmployees() { // GET
                try {
                    const response = await fetch('https://jsonplaceholder.typicode.com/users')
                    const data = await response.json()
                    this.employees = data
                } catch (error) {
                    console.error(error)
                }
            },
            async addEmployee(employee) { // POST
                try {
                    const response = await fetch('https://jsonplaceholder.typicode.com/users', {
                        method: 'POST',
                        body: JSON.stringify(employee),
                        headers: {'Content-type': 'application/json; charset=UTF-8'},
                    })
                    const data = await response.json()
                    this.employees = [...this.employees, data]
                } catch (error) {
                    console.error(error)
                }
            },
            async editEmployee(id, updatedEmployee) { // PUT
                try {
                    const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
                        method: 'PUT',
                        body: JSON.stringify(updatedEmployee),
                        headers: {'Content-type': 'application/json; charset=UTF-8'},
                    })
                    const data = await response.json()
                    this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
                } catch (error) {
                    console.error(error)
                }
            },
            async deleteEmployee(id) { // DELETE
                try {
                    await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
                        method: "DELETE"
                    });
                    this.employees = this.employees.filter(employee => employee.id !== id);
                } catch (error) {
                    console.error(error);
                }
            }
        },
        mounted() {
            this.getEmployees()
        },
    }
</script>

<style>
    button {
        background: #009435;
        border: 1px solid #009435;
    }
    button:hover,
    button:active,
    button:focus {
        background: #32a95d;
        border: 1px solid #32a95d;
    }
    .small-container {
        max-width: 680px;
    }
</style>
