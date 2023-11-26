<script>
import {onMount} from 'svelte'
//array untuk menampung data karyawan
let datas = []
//state/ memory untuk menangani modals
let modalState = {
    show:false,
    defaultValue : {}
}



function getDataKaryawan (){
    fetch ("http://localhost:3000/data_karyawan", {
        method : "GET",
        headers : {
            "Content-Type" : "application/json"
        }
    })
    .then(res => res.json())
    .then(data =>{
        datas = data;
    })
}

//menghapus data karyawan
function deleteDataKaryawan(id){
    let conf = window.confirm ("Yakin delete data ini ?")
    if(!conf){
       return
    }
    fetch (`http://localhost:3000/data_karyawan/${id}`,{
        method : "DELETE",
        headers : {
            "Content-Type" : "application/json"
        }
    })
    .then(res => res.json())
    .then(data => {
        window.location.reload()
    })
}

//show modal data karyawan
function showModalKaryawan(data){
 modalState ={
    show:true,
    defaultValue:data
 }
}

function handleCancel(){
    modalState = {
        show:false,
        defaultValue :{}
    }
}

function handleUpdateData(event){
    //stop form reload page
    event.preventDefault();
    fetch(`http://localhost:3000/data_karyawan/${modalState.defaultValue.id}`,{
        method : "PUT",
        headers :{
            "Content-Type" : "application/json"
        },
        body : JSON.stringify({
            fullname : modalState.defaultValue.fullname,
            email : modalState.defaultValue.email,
            phone : modalState.defaultValue.phone,
        })
    })
    .then(res => res.json())
    .then(data => {
        window.location.reload()
    })
}

//fungsi yang berjalam setelah component di mount/tampil di browser
onMount(()=>{
    getDataKaryawan();
})

//side effect
$:{
    console.info(datas)
}
</script>

<table class="w-full mt-2">
    <thead>
        <tr>
            <th class="border bg-orange-500 text-white p-2">No</th>
            <th class="border  bg-orange-500 text-white p-2 w-3/12">Fullname</th>
            <th class="border  bg-orange-500 text-white p-2 w-3/12">Email</th>
            <th class="border  bg-orange-500 text-white p-2">phone</th>
            <th class="border  bg-orange-500 text-white p-2 bg-gray-500 w-3/12">Action</th>
        </tr>
    </thead>
    <tbody>
        {#each datas as dt (dt.id) }
        <tr class={`hover:bg-slate-200`}>
            <td class={`border-p2 text-center`}>{datas.indexOf(dt) +1}</td>
            <td class={`border-p2`}>{dt.fullname}</td>
            <td class={`border-p2`}>{dt.email}</td>
            <td class={`border-p2`}>{dt.phone}</td>
            <td class={`border-p2 text-center`}>
            <button class={`p-2 w-32 bg-green-500 text-white`}
            on:click={()=>{showModalKaryawan(dt)}}
            >
            edit
            </button>
            <button class={`p-2 w-32 bg-red-500 text-white`}
            on:click={()=>{deleteDataKaryawan(dt.id)}}
            >
                delete
            </button>
    
            </td>

        </tr>
        {/each}
    </tbody>
</table>
<!-- Buat Form untuk edit -->
{#if modalState.show}
<div class="flex flex-col w-screen h-screen bg-black/50 fixed justify-center items-center">
    <form class={`flex flex-col bg-white p-4 gap-4 w-[300px]`} on:submit={handleUpdateData}>
        <div class="flex flex-col">
            <label for="fullname">Fullname</label>
            <input type="text" id="fullname" class="border" bind:value={modalState.defaultValue.fullname}>
        </div>
        <div class="flex flex-col">
            <label for="email">Email</label>
            <input type="email" id="email" class={`border`} bind:value={modalState.defaultValue.email}>    
        </div>
        <div class="flex flex-col">
            <label for="phone">Phone</label>
            <input type="tel" id="phone" class={`border`} bind:value={modalState.defaultValue.phone}>
        </div>
        <div class={`flex gap-2`}>
            <button class={`bg-red-500 text-white flex-1 rounded-md p-2`} type="button" 
            on:click={handleCancel}
            >Cancel</button>
            <button class={`bg-blue-500 text-white flex-1 rounded-md p-2`}>Save</button>
        </div>
    </form>
</div>
{/if}