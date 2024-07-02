# **Studi Kasus Arsitektur Saga**
## **Kebutuhan tiap service**
1. Layanan Pengguna (User Service)
   1. Registrasi Pengguna

      Menambahkan Pengguna baru sesuai data yang dibutuhkan ke database.

   1. Management Profil

      ~~C~~RUD : ambil data user, Update data user, hapus data user. Login/autentikasi user. 

1. Layanan Event (Event Service)
   1. Pembuatan dan management Event (Admin)

      CRUD = Admin bisa membuat event, admin bisa mengedit event, admin bisa menghapus event

   1. Melihat daftar event yang tersedia (User)
      1. User bisa melihat semua event
      1. User bisa melihat detail sebuah event
1. Layanan Pemesanan Tiket
   1. Pengguna dapat memesan ticket untuk event
      1. Hanya create berdasarkan event yg tersedia, kuota yang tersedia, jumlah ticket yang dipesan, dan siapa yang memesan
   1. Pengguna dapat membatalkan pemesanan ticket
      1. Edit status ticket yang sudah dipesan berdasarkan idTicket. Sistem/service lain nantinya akan mengembalikan uang yang sudah dibayar (dikurangi fee pembatalan), dan menambahkan kembali jumlah tiket yang tersedia.
1. Layanan Pembayaran
   1. Proses pembayaran untuk pemesanan ticket


## **Komponen dan Fungsi**
1. Layanan Pengguna
   1. Registrasi Pengguna

      Component:

- Domain Layer
  - Entity : userId, userName, name, email, address
  - Repository interface : save()
- Application Layer
  - Service: createUserService
  - DTO: registerDTO
  - API RestController : RestAPI post(“user”);
- Infrastructure Layer\
  - UI
  - Repository implementations
  1. Management Profil

     Component:

     1. ` `Domain Layer
        1. Entity : userId, userName, name, email, address
        1. Repository interface : save()
        1. Domain Service: getUserById, editUser
     1. Application Layer
        1. Service: authUser
        1. DTO: loginDTO
        1. API RestController : RestAPI post(“user/login”); get(“user/{id}); put(“user/{id}”);
     1. Infrastructure Layer\
        1. UI
        1. Repository implementations
        1. Kafka untuk mengirim event ke service setelah ini

Fungsi:

1. authUser
   1. Check data (username & password dengan data di database)
   1. Buat jwt
   1. Kirim sebagai response



1. Layanan Event
   1. Pembuatan dan Management Event

      Component:

      1. Domain Layer
         1. Entity : eventId, eventName , creator, address, time
         1. Repository interface : save(), getAllEvent, getEventById, delete
         1. Domain Service: getEvents, getEventById, 
      1. Application Layer
         1. Service: authorizeUser
         1. DTO: eventDTO
         1. API RestController : RestAPI post(“event”); get(“event”); get(“event/{id}”); put(“event/{id}”);
      1. Infrastructure Layer\
         1. UI
         1. Repository implementations
         1. Kafka untuk mengirim event ke service setelah ini
1. Pemesanan Tiket
   1. Pemesanan Tiket Event

      Component:

      1. Domain Layer
         1. Entity : eventId, ticketId, userId
         1. Repository interface : save()
      1. Application Layer
         1. Service: createOrder, cancelOrder
         1. DTO: ticketDto
         1. API RestController : RestAPI post(“ticket”); put(“ticket/{id}”);
      1. Infrastructure Layer\
         1. UI
         1. Repository implementations
         1. Kafka untuk mengirim event ke service setelah ini





1. Layanan Pembayaran
   1. Proses pembayaran untuk pemesanan tiket.

      Component:

      1. Domain Layer
         1. Entity : ticketId, methodId, userId
         1. Repository interface : save()
      1. Application Layer
         1. Service: createPayment, 
      1. Infrastructure Layer\
         1. UI
         1. Repository implementations
         1. Kafka untuk mengirim event ke service setelah ini
         1. API untuk ke payment services

