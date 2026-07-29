zenuml
title E-Commerce - Checkout Process

@Actor Customer
@Boundary WebApp
@Control AuthService
@Control PaymentGateway
@Control NotificationService
@Database Database

Customer->WebApp: Checkout()

WebApp.checkout(username, password, productId, qty) {

    // Login
    user = AuthService.login(username, password)

    if(user == null) {
        WebApp->Customer: Login Gagal
        return
    }

    // Cek Produk
    product = Database.getProduct(productId)

    if(product == null) {
        WebApp->Customer: Produk Tidak Ditemukan
        return
    }

    // Cek Stok
    if(product.stock < qty) {
        WebApp->Customer: Stok Tidak Mencukupi
        return
    }

    // Hitung Total
    total = product.price * qty

    // Pembayaran
    payment = PaymentGateway.pay(total)

    if(payment.success) {

        Database.beginTransaction()

        Database.saveOrder(user.id, productId, qty, total)
        Database.reduceStock(productId, qty)

        Database.commit()

        NotificationService.sendEmail(user.email)

        WebApp->Customer: Checkout Berhasil

    } else {

        Database.rollback()

        WebApp->Customer: Pembayaran Gagal

    }

}
