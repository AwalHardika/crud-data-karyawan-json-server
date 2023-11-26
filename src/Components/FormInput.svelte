<script>
    let fullname =""
    let email = ""
    let phone = ""
    
    //fungsi untuk mengirim data ke backend
    function handleSubmit(event){
    if (!fullname || !email || !phone) {
        alert("Data tidak boleh kosong")
        return
    }

    if(fullname.length<= 5){
       alert("Masukan nama dengan benar")
        return
    }
    if(!email.includes("@")){
        return
    }
    if(phone.length<=8){
        alert("Masukan telepon fengan benar")
        return
    }
    //stop form dari reload
    event.preventDefault()
    fetch("http://localhost:3000/data_karyawan",{
      method : "POST", 
      headers: {
        "Content-Type" : "application/json"
      },
      body : JSON.stringify({
        fullname : fullname,
        email : email,
        phone : phone
      })
    })
    .then(res => res.json())
    .then(data =>{
      console.info(data);
    fullname=""
    email = ""
    phone = ""
    alert("Data Karyawan Berhasil ditambah")
    //reload page
    window.location.reload()
    })
    
    }
    //side effect
    // $:{
    //   console.log({fullname,email,phone})
    // }
    </script>

<form class="flex gap-2 items-end" on:submit={handleSubmit}>
    <div>
      <label for="fullname">fullname</label>
      <input type="text" id="fullname" class={`p-2 border-gray-300 border-2`} autocomplete="off" 
      bind:value={fullname} required
      >
    </div>
    <div>
      <label for="email">Email</label>
      <input type="email" id="email" class={`p-2 border-gray-300 border-2`} autocomplete="off" 
      bind:value={email} required
      >
    </div>
    <div>
      <label for="phone">Phone</label>
      <input type="tel" id="phone" class={`p-2 border-gray-300 border-2`} autocomplete="off"
      bind:value={phone} required
      >
    </div>

    <button class={`p-2 bg-orange-500 text-white w-32 rounded-sm`}>
    submit      
    </button>
  </form>