# Database

Database menyimpan data aplikasi secara terorganisir. Aplikasi tersebut kemudian mengeluarkan query untuk mengambil bagian tertentu dari data sesuai kebutuhan. Basis data yang paling umum digunakan untuk aplikasi web adalah yang didasarkan pada model relasional, juga disebut basis data SQL mengacu pada Structured Query Language yang mereka gunakan. Tetapi dalam beberapa tahun terakhir, basis data yang berorientasi pada dokumen dan nilai kunci, yang secara informal dikenal bersama sebagai basis data NoSQL, telah menjadi alternatif yang populer.

## SQL Databases

Database relasional menyimpan data dalam tabel, yang memodelkan berbagai entitas dalam domain aplikasi. Misalnya, database untuk aplikasi manajemen pesanan kemungkinan akan memiliki tabel pelanggan, produk, dan pesanan. Tabel memiliki jumlah kolom tetap dan jumlah baris variabel. Kolom mendefinisikan atribut data entitas yang diwakili oleh tabel. Misalnya, tabel pelanggan akan memiliki kolom seperti nama, alamat, telepon, dan sebagainya. Setiap baris dalam sebuah tabel mendefinisikan elemen data aktual yang memberikan nilai pada beberapa atau semua kolom. Tabel memiliki kolom khusus yang disebut kunci utama, yang berisi pengidentifikasi unik untuk setiap baris yang disimpan dalam tabel. Tabel juga dapat memiliki kolom yang disebut kunci asing, yang merujuk primary key dari suatu baris dalam tabel yang sama atau yang lain. Tautan ini antara baris disebut hubungan dan merupakan dasar dari basis data relasional model.

 

![Relasi Tabel roles dan users](https://lh4.googleusercontent.com/lZTfJmRzo8WaOcJh-Jqt23nLuqkmb6bkCRwEQ6rfwQFn97q-8a50HuhdJKkynLrLa6a18zzBcYi9tt3sofEFIKTmzdYc9laZiK-5kEllJVigK61exDefEL8hBUrl4N-SwnCMnDrQ)

## Python Database Framework

Python memiliki paket untuk sebagian besar mesin basis data, baik sumber terbuka maupun komersial. Flask tidak membatasi paket database apa yang dapat digunakan, sehingga Anda dapat bekerja dengan MySQL, Postgres, SQLite, Redis, MongoDB, CouchDB, atau DynamoDB. Ada sejumlah faktor yang harus dievaluasi saat memilih database framework:

1. Kemudahan penggunaan
2. Performa
3. Portabilitas
4. Integrasi Flask

## Database Management Menggunakan Flask SQLAlchemy

Flask-SQLAlchemy adalah ekstensi Flask yang menyederhanakan penggunaan SQLAlchemy di dalam aplikasi Flask. SQLAlchemy adalah kerangka kerja basis data relasional yang kuat yang mendukung beberapa backend basis data. Ini menawarkan ORM tingkat tinggi dan akses tingkat rendah ke fungsionalitas SQL database. Seperti kebanyakan extensi flask lainnya, Flask-SQLAlchemy di install melalui pip:

```text
(env) $ pip install flask-sqlalchemy
```

Untuk menghubungkan Flask-SQLAlchemy dengan database server, dibutuhkan driver. Berikut ini cara install driver tersebut:

**DBMS MySQL**

PyMySQL Cilent driver di Linux/Mac/Windows:

```text
(env) $ pip install pymysql
```

MySQL Cilent driver di Linux/Mac/Windows:

```text
(env) $ pip install mysqlclient
```

**DBMS Postgres**

Driver di Linux/Mac/Windows:

```text
(env) $ pip install psycopg2
```

String koneksi pada konfigurasi database :

| Database Engine | URL |
| :--- | :--- |
| MySQL - pymysql | mysql+pymysql://username:password@hostname/database |
| MySQL - mysqlclient | mysql://username:password@hostname/database |
| PostgreSQL | postgresql://username:password@hostname/database |
| SQL \(Linux, MacOS\) | sqlite:////absolute/path/to/database |
| SQL \(Windows\) | sqlite:///c:/absolute/path/to/database |

Cara konfigurasi Flask SQLAlchemy, buka app.py dan tambahkan kode ini:

```python
# app.py: inisisalisasi dan koneksi
...
from wtforms.validators import DataRequired
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
moment = Moment(app)
db = SQLAlchemy(app)

app.config['SECRET_KEY'] = 'thisisverysecret'

app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://username:password@localhost/nama-database'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
...
```

Objek db dipakai dari kelas SQLAlchemy mewakili database dan menyediakan akses ke semua fungsi Flask-SQLAlchemy.

## Pendefinisian Model

Model biasanya python class dengan atribut yang cocok dengan kolom tabel database. Tulislah sebuah kode di app.py:

```python
# app.py: membuat tabel User dan Role
...
from flask_sqlalchemy import SQLAlchemy

...
# Forms
class UserForm(FlaskForm):
    name = StringField('Siapa nama kamu?', validators=[DataRequired()])
    submit = SubmitField('Lanjutkan')

# Model - Tambahkan Ini
class Role(db.Model):
    __tablename__ = 'roles'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)
    
    def __repr__(self):
        return '<Role {}>'.format(self.name)

class User(db.Model):
    __tablename__ = 'users'
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, index=True)
    
    def __repr__(self):
        return '<User {}>'.format(self.username)
```

Variabel kelas `__tablename__` mendefinisikan nama tabel dalam database. Flask-SQLAlchemy memberikan nama tabel default jika `__tablename__` dihilangkan, tetapi nama-nama default itu tidak mengikuti konvensi populer menggunakan bentuk jamak untuk nama tabel, jadi yang terbaik adalah memberi nama tabel secara eksplisit. Variabel kelas yang tersisa adalah atribut model seperti db.Integer, didefinisikan sebagai turunan dari kelas db.Column. Metode **repr** memberi representasi string agar memudahkan pengujian dan debugging.

Tipe kolom populer dari SQLAlchemy

| Nama Tipe | Tipe Python |
| :--- | :--- |
| Integer | int |
| SmallInteger | int |
| BigInteger | int or long |
| Float | float |
| Numeric | decimal.Decimal |
| String | str |
| Text | str |
| Unicode | unicode |
| UnicodeText | unicode |
| Boolean | bool |
| Date | datetime.date |
| Time | datetime.time |
| DateTime | datetime.datetime |
| Interval | datetime.timedelta |
| Enum | str |
| PickelType | object python |
| LargeBinary | str |

Tipe pilihan kolom populer dari SQLAlchemy

| Nama | Deskripsi |
| :--- | :--- |
| primary\_key | Jika nilainya True, maka kolom tabel ini primary key |
| unique | Jika nilainya True, maka tidak boleh memiliki nilai yang sama |
| index | Jika nilainya True, maka kolom akan terindex |
| nullable | Jika nilainya True, maka kolom ini bisa kosong, dan sebaliknya |
| default | Memberikan nilai default pada kolom |

## Relasi Database

Database relasional membuat koneksi antara baris dalam tabel yang berbeda. Pada `app.py` tambahkan kode berikut ini:

```python
# app.py: menambahkan relasi
class Role(db.Model):
    ...
    users = db.relationship('User', backref='role')

class User(db.Model):
    ...
    role_id = db.Column(db.Integer, db.ForeignKey('roles.id')

...
```

Ini adalah hubungan satu-ke-banyak dari role ke user, karena satu role dapat memiliki banyak user, tetapi setiap user hanya dapat memiliki satu peran. Hubungan antara dua tabel tersebut dihubungkan oleh foreign key. Kolom `role_id` yang ditambahkan ke User didefinisikan sebagai foreign key.

| Nama | Deskripsi |
| :--- | :--- |
| `backref` | Mengembalikkan referensi dalam model yang berhubungan |
| `primaryjoin` | Menentukan kondisi antara kedua model secara eksplisit. Ini diperlukan hanya untuk hubungan yang ambigu. |
| `lazy` | Tentukan bagaimana item terkait dimuat. Nilai yang mungkin dipilih \(item dimuat menuntut saat pertama kali diakses\), langsung \(item dimuat ketika objek sumber adalah dimuat\), bergabung \(item dimuat segera, tetapi sebagai gabungan\), subquery \(item dimuat segera, tetapi sebagai subquery\), noload \(item tidak pernah dimuat\), dan dinamis \(bukan memuat item, kueri yang dapat memuatnya diberikan\). |
| `uselist` | Jika disetel ke False, gunakan skalar dari pada _List_. |
| `order_by` | Menentukan urutan list dalam model |
| `secondary` | Menentukan tabel asosiasi untuk hubungan banyak ke banyak |
| `secondaryjoin` | Menentukan tabel asosiasi untuk hubungan banyak ke banyak saat SQLAlchemy tidak dapat menentukan dirinya sendiri |

Ada jenis hubungan lain selain satu ke banyak. Hubungan one-to-many dapat diekspresikan dengan cara yang sama dengan hubungan satu-ke-banyak. Hubungan many-to-one juga dapat dinyatakan sebagai hubungan satu-ke-banyak jika tabel dibalik, atau dapat diekspresikan dengan kunci asing dan definisi `db.relationship()` baik pada sisi "banyak". Itu tipe hubungan paling kompleks, many-to-many, memerlukan tabel tambahan yang disebut asosiasi.

## Operasi Database

Cara terbaik untuk belajar bagaimana bekerja dengan model yang sudah dibuat adalah dengan Python Shell. Cara menggunakannya dengan menggunakan flask shell. Namun, sebelum menggunakan flask shell pastikan environment variable kamu `FLASK_APP` diatur ke `app.py`.

### Membuat Database & Tabel

Hal pertama yang kamu harus lakukan adalah **membuat sebuah database bernama flask\_web\_development** di mysql kamu. Fungsi `db.create_all()` menempatkan semua subclass db.Models dan membuat tabel terkait dalam model.

```text
(env) $ flask shell
>>> from app import db
>>> db.create_all() 
```

Perintah dari `flask shell` `db.create_all()` di atas akan membuat tabel dari masing-masing kelas model ke database yang sudah kamu buat. Dan cara menghapus semua tabel yang sudah kita buat dengan cara:

```text
>>> db.drop_all()
>>> db.create_all()
```

Cara di atas akan menghapus seluruh data kamu dari database.

### Insert Data Melalui Flask Shell

Cara memasukkan data melalui flask-shell, pertama jalankan dahulu `flask shell`:

```text
>>> from app import Role, User, db
>>> admin_role = Role(name='Admin')
>>> mod_role = Role(name='Moderator')
>>> user_role = Role(name='User')
>>> user_david = User(username='david', role=admin_role)
>>> user_teguh = User(username='teguh', role=user_role)
>>> user_sabil = User(username='sabil', role=user_role)
```

Objek hanya ada di sisi Python, nilai-nilai diatas belum ditulis ke database. Perubahan pada database dikelola melalui database session, SQLAlchemy menyediakannya sebagai db.session. Untuk melakukannya cukup seperti ini:

```text
>>> db.session.add(admin_role)
>>> db.session.add(mod_role)
>>> db.session.add(user_role)
>>> db.session.add(user_david)
>>> db.session.add(user_teguh)
>>> db.session.add(user_sabil)
```

atau lebih singkat dengan:

```text
>>> db.session.add_all([admin_role, mod_role, user_role, user_david, user_teguh, user_sabil])
```

Kemudian untuk memasukkannya ke database, session tadi harus di commit dengan memanggil commit\(\) method:

```text
>>> db.session.commit()
```

### Edit Data

`add()` method seperti yang di atas bisa juga digunakan untuk melakukan edit atau update data. Kamu coba untuk merubah data admin di shell yang sama:

```text
>>> admin_role.name = 'Administrator'
>>> db.session.add(admin_role)
>>> db.session.commit()
```

### Hapus Data

Database session memiliki `delete()` method. Semisal kamu akan menghapus Moderator role dari database:

```text
>>> db.session.delete(mod_role)
>>> db.session.commit()
```

Inget iya, setiap melakukan `add()` dan `delete()` semuanya harus di `commit()` untuk perubahan di database.

### Query Data

Flask-SQLAlchemy bisa memperlihatkan semua data pada database dengan metode `all()` `flask shell`

```text
>>> Role.query.all()
[<Role Administrator’>, <Role User>]
>>> User.query.all()
[<User david>, <User teguh>, <User sabil>]
```

Flask-SQLAlchemy juga bisa memperlihatkan semua data pada database dengan mem-filter data pada database dengan `filter_by()`:

```text
>>> User.query.filter_by(role=user_role).all()
[<User teguh>, <User sabil>]
```

Kamu juga bisa untuk mengecek query SQL yang dihasilkan SQLALchemy:

```text
>>> str(User.query.filter_by(role=user_role))
'SELECT users.id AS users_id, users.username AS users_username, users.role_id AS users_role_id \nFROM users \nWHERE %(param_1)s 
= users.role_id'
```

Jika kamu keluar dari `flask-shell` maka objek-objek yang sebelumnya akan hilang selain yang sudah di commit. Data yang sudah dicommit akan tersimpan di database. Jika kamu membuka shell baru maka kamu kamu harus import ulang dari app. Nah, jika kamu ingin mengambil data pertama dari database maka gunakan metode `first()`:

```text
>>> user_role = Role.query.filter_by(name='User').first()
```

metode `first()` mengambil data pertama dari id tabel Role.

Beberapa perintah query filters:

| Option | Deksripsi |
| :--- | :--- |
| `filter()` | Memfilter sesuai |
| `filter_by()` | Memfilter dengan nilai kesamaan |
| `limit()` | Membatasi jumlah hasil query |
| `offset()` | Menerapkan offset ke hasil query |
| `order_by()` | Mengurutkan data sesuai dengan nilai yang diberikan |
| `group_by()` | Mengelompokkan hasil data sesuai dengan nilai yang diberikan |

Beberapa perintah query SQLAlchemy:

| Option | Deskripsi |
| :--- | :--- |
| `all()` | Mengeluarkan seluruh data |
| `first()` | Mengeluarkan data pertama |
| `first_or_404()` | Mengelurakan data pertama, namun jika data tidak ada akan mengirim 404 sebagai respon |
| `get()` | Mengembalikan data sesuai id yang diberikan |
| `get_or_404()` | Mengembailkan data sesuai id yang diberikan, namun jika data tidak ada akan mengirim 404 sebagai respon |
| `count()` | Menghitung data |
| `paginate()` | Membuat paginate sesuai range |

SQLAlchemy relationship mirip dengan query. Contoh hubungan one-to-many antara role dan user:

```text
>>> users = user_role.users
>>> users
[<User teguh>, <User sabil>]
>>> users[0].role
<Role User>
```

`lazy='dynamic'` berfungsi untuk query tidak dieksekusi secara otomatis:

```python
# app.py
...
# Model
class Role(db.Model):
    __tablename__ = 'roles'
    ...
    users = db.relationship('User', backref='role', lazy='dynamic')

...
```

dengan relasi seperti ini, jadi filter bisa ditambahkan, tapi sebelum menjalankan perintah di bawah pastikan kamu reload flask shell kamu:

```text
>>> user_role.users.order_by(User.username).all()
[<User david>, <User susan>]
>>> user_role.users.count()
2
```

## Menggunakan Fungsi Database Pada View

Perintah-perintah pada flask shell yang sebelumnya dapat kamu gunakan pada fungsi route.

```python
# app.py: menambahkan perintah untuk insert ke db
...
@app.route('/'. methods=['GET', 'POST'])
def index():
    form = UserForm()
    if form.validate_on_submit():
        user=User.query.filter_by(username=form.name.data).first()
        if user is None:
            user = User(username=form.name.data)
            db.session.add(user)
            db.session.commit()
            flash('Senang bertemu kamu')
        else:
            flash('Eh, kita ketemu lagi dong!')
        session['name'] = form.name.data
        return redirect(url_for('user', name=session.get('name')))
    return render_template('index.html', form=form, name=session.get('name'))
...
```

Route ini memeriksa dalam database menggunakan `filter_by()`. Variabel _known_ ditulis ke sesi user yang ke redirect ke template. Ketika user tidak ada maka akan membuat user baru dengan nama yang sudah di inputkan ke form.

## Integrasi dengan Python Shell

Integrasi ini berfungsi ketika kamu menjalani flask shell, dengan integrasi kamu tidak lagi mengimport objek yang akan digunakan. Nah, kamu akan menggunakan app.shell\_context\_processor decorator.

Taruh kode ini di paling bawah:

```python
# app.py: integrasi dengan python shell
...
@app.shell_context_processor
def make_shell_context():
    return dict(db=db, User=User, Role=Role)
```

Fungsi shell\_context\_processor mengembalikan hasil dict kedalam flask shell.

```text
$ flask shell
>>> app
<Flask 'app'>
>>> db
<SQLAlchemy engine=mysql+pymysql://root:***@localhost/flask_web_development?charset=utf8>
>>> User
<class 'app.User'>
>>> Role
<class 'app.Role'>
```

## Migrasi database dengan Flask Migrate

Migrasi merupakan langkah yang mempermudah dalam proses pembaharuan model data, sehingga tidak perlu secara manual melakukan update tabel pada database.

### Membuat Repository Migrasi

Lakukan instalasi ekstensi Flask-Migrate pada flask menggunakan pip dengan cara:

```text
(venv) $ pip install flask-migrate
```

Selanjutnya inisialisasi flask-migrate dengan cara:

```python
# app.py: menambahkan inisialisasi
...
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

...
db = SQLAlchemy(app)
migrate = Migrate(app, db)
...
```

Kemudian, gunakan flask db init untuk inisialisasi db:

```text
(env) $ flask db init
Creating directory C:\Users\david\Documents\flask-web-development\migrations ...  done
Creating directory C:\Users\david\Documents\flask-web-development\migrations\versions ...  done
Generating C:\Users\david\Documents\flask-web-development\migrations\alembic.ini ...  done
Generating C:\Users\david\Documents\flask-web-development\migrations\env.py ...  done
Generating C:\Users\david\Documents\flask-web-development\migrations\README ...  done
Generating C:\Users\david\Documents\flask-web-development\migrations\script.py.mako ...  done
Please edit configuration/connection/logging settings in 'C:\\Users\\david\\Documents\\flask-web-development\\migrations\\alembic.ini' before proceeding.
```

Perintah ini membuat folder migrations, yang dimana isinya merupakan migration scripts.

### Membuat Script Migrasi

Membuat script migrasi yang ditaruh di folder migrasi.

```text
(env) $ flask db migrate -m 'initial migration'
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.env] No changes in schema detected.
```

### Upgrade Database

Setelah migrations script di migrate, perlu dilakukan flask db upgrade untuk diaplikasikan ke database:

```text
(env) $ flask db upgrade
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
```

Ini sama ketika kita menggunakan `db.create_all()`. Namun, `flask db upgrade` merubah tabel tanpa merubah content dalam tabel.

