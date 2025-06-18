# FRONTEND 
```
php artisan make:controller MahasiswaController
php artisan make:controller DosenController
```

1. APP > Http > Controllers
- DosenWaliController.php
```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Http;

class DosenWaliController extends Controller
{
    public function index()
    {
        $response = Http::get('http://localhost:8080/dosen');
        $dosen_wali = $response->json();
        return view('dosen_wali.index', compact('dosen_wali'));
    }

    public function create()
    {
        return view('dosen_wali.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'nama_dosen' => 'required|string|max:50',
            'nidn' => 'required|string|max:15',
            'id_user' => 'required|integer',
        ]);

        $response = Http::asJson()->post('http://localhost:8080/dosen', [
            'nama_dosen' => $request->nama_dosen,
            'nidn' => $request->nidn,
            'id_user' => $request->id_user,
        ]);

        if ($response->successful()) {
            return redirect()->route('dosen_wali.index')->with('success', 'Berhasil menambahkan dosen.');
        }

        return back()->withErrors(['error' => 'Gagal menambahkan data'])->withInput();
    }

    public function edit($id_dosen)
    {
        $response = Http::get("http://localhost:8080/dosen/$id_dosen");

        if ($response->successful()) {
            $dosen = $response->json()[0];
            return view('dosen_wali.edit', compact('dosen'));
        }

        return back()->with('error', 'Data tidak ditemukan.');
    }

    public function update(Request $request, $id)
    {
        $request->validate([
            'nama_dosen' => 'required|string|max:50',
            'nidn' => 'required|string|max:15',
            'id_user' => 'required|integer',
        ]);

        $response = Http::put("http://localhost:8080/dosen/$id", $request->all());

        if ($response->successful()) {
            return redirect()->route('dosen_wali.index')->with('success', 'Berhasil mengupdate data.');
        }

        return back()->withErrors(['error' => 'Gagal mengupdate data'])->withInput();
    }

    public function destroy($id)
    {
        $response = Http::delete("http://localhost:8080/dosen/$id");

        if ($response->successful()) {
            return redirect()->route('dosen_wali.index')->with('success', 'Berhasil menghapus data.');
        }

        return back()->with('error', 'Gagal menghapus data.');
    }
}
```

- MahasiswaConrtoller.php
```
<?php

namespace App\Http\Controllers;


use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Http;

class MahasiswaController extends Controller
{
    public function index()
    {
        $response = Http::get('http://localhost:8080/mahasiswa');
        $mhs = $response->json();
        return view('mahasiswa.index', compact('mhs'));
    }

    public function create()
    {
        return view('mahasiswa.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'npm' => 'required|integer',
            'id_user' => 'required|integer',
            'id_dosen' => 'required|integer',
            'id_kajur' => 'required|integer',
            'nama_mahasiswa' => 'required|string|max:50',
            'tempat_tanggal_lahir' => 'required|string|max:50',
            'jenis_kelamin' => 'required|string|max:10',
            'alamat' => 'required|string|max:100',
            'agama' => 'required|string|max:20',
            'angkatan' => 'required|integer',
            'program_studi' => 'required|string|max:20',
            'semester' => 'required|integer',
            'no_hp' => 'required|string|max:15',
            'email' => 'required|email',

        ]);

        $response = Http::asJson()->post('http://localhost:8080/mahasiswa', [
            'npm' => $request->npm,
            'id_user' => $request->id_user,
            'id_dosen' => $request->id_dosen,
            'id_kajur' => $request->id_kajur,
            'nama_mahasiswa' => $request->nama_mahasiswa,
            'tempat_tanggal_lahir' => $request->tempat_tanggal_lahir,
            'jenis_kelamin' => $request->jenis_kelamin,
            'alamat' => $request->alamat,
            'agama' => $request->agama,
            'angkatan' => $request->angkatan,
            'program_studi' => $request->program_studi,
            'semester' => $request->semester,
            'no_hp' => $request->no_hp,
            'email' => $request->email
        ]);

        if ($response->successful()) {
            return redirect()->route('mahasiswa.index')->with('success', 'Berhasil menambahkan mahasiswa.');
        }

        return back()->withErrors(['error' => 'Gagal menambahkan data'])->withInput();
    }

    public function edit($npm)
    {
        $response = Http::get("http://localhost:8080/mahasiswa/$npm");

        if ($response->successful()) {
            $mahasiswa = $response->json()[0];
            return view('mahasiswa.edit', compact('mahasiswa'));
        }

        return back()->with('error', 'Data tidak ditemukan.');
    }

    public function update(Request $request, $npm)
    {
        $request->validate([
            'npm' => 'required|integer',
            'id_user' => 'required|integer',
            'id_dosen' => 'required|integer',
            'id_kajur' => 'required|integer',
            'nama_mahasiswa' => 'required|string|max:50',
            'tempat_tanggal_lahir' => 'required|string|max:50',
            'jenis_kelamin' => 'required|string|max:10',
            'alamat' => 'required|string|max:100',
            'agama' => 'required|string|max:20',
            'angkatan' => 'required|integer',
            'program_studi' => 'required|string|max:20',
            'semester' => 'required|integer',
            'no_hp' => 'required|string|max:15',
            'email' => 'required|email',
        ]);

        $response = Http::put("http://localhost:8080/mahasiswa/$npm", $request->all());

        if ($response->successful()) {
            return redirect()->route('mahasiswa.index')->with('success', 'Berhasil mengupdate data.');
        }

        return back()->withErrors(['error' => 'Gagal mengupdate data'])->withInput();
    }

    public function destroy($npm)
    {
        $response = Http::delete("http://localhost:8080/mahasiswa/$npm");

        if ($response->successful()) {
            return redirect()->route('mahasiswa.index')->with('success', 'Berhasil menghapus data.');
        }

        return back()->with('error', 'Gagal menghapus data.');
    }
}
```

2. Resources > Views
- Mahasiswa
1. Index.php 
```
 @extends('layouts.app') 

 @section('content') 
<div class="container">
    <h1>Daftar Mahasiswa</h1>
    <a href="{{ route('mahasiswa.create') }}" class="btn btn-primary mb-3">Tambah Mahasiswa</a>

    @if (session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    <table class="table table-bordered">
        <thead>
            <tr>
                <th>npm</th>
                <th>id_user</th>
                <th>id_dosen</th>
                <th>id_kajur</th>
                <th>nama_mahasiswa</th>
                <th>tempat_tanggal_lahir</th>
                <th>jenis_kelamin</th>
                <th>alamat</th>
                <th>agama</th>
                <th>angkatan</th>
                <th>program_studi</th>
                <th>semester</th>
                <th>no_hp</th>
                <th>email</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($mhs as $mahasiswa)
            <tr>
                <td>{{ $mahasiswa['npm'] }}</td>
                <td>{{ $mahasiswa['id_user'] }}</td>
                <td>{{ $mahasiswa['id_dosen'] }}</td>
                <td>{{ $mahasiswa['id_kajur'] }}</td>
                <td>{{ $mahasiswa['nama_mahasiswa'] }}</td>
                <td>{{ $mahasiswa['tempat_tanggal_lahir'] }}</td>
                <td>{{ $mahasiswa['jenis_kelamin'] }}</td>
                <td>{{ $mahasiswa['alamat'] }}</td>
                <td>{{ $mahasiswa['agama'] }}</td>
                <td>{{ $mahasiswa['angkatan'] }}</td>
                <td>{{ $mahasiswa['program_studi'] }}</td>
                <td>{{ $mahasiswa['semester'] }}</td>
                <td>{{ $mahasiswa['no_hp'] }}</td>
                <td>{{ $mahasiswa['email'] }}</td>
                <td>
                    <a href="{{ route('mahasiswa.edit', $mahasiswa['npm']) }}" class="btn btn-sm btn-warning">Edit</a>
                    <form method="POST" action="{{ route('mahasiswa.destroy', $mahasiswa['npm']) }}" style="display:inline;">
                        @csrf
                        @method('DELETE')
                        <button onclick="return confirm('Yakin ingin hapus?')" class="btn btn-sm btn-danger">Hapus</button>
                    </form>
                </td>
            </tr>
            @endforeach
        </tbody>
    </table>
</div>
 @endsection 
```

2. edit.blade.php
```
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Edit Mahasiswa</h1>
    <form method="POST" action="{{ route('mahasiswa.update', $mahasiswa['npm']) }}">
        @csrf
        @method('PUT')
        <div class="mb-3">
            <label>NPM</label>
            <input type="text" name="npm" class="form-control" value="{{ $mahasiswa['npm'] }}" required>
        </div>
        <div class="mb-3">
            <label>ID User</label>
            <input type="number" name="id_user" class="form-control" value="{{ $mahasiswa['id_user'] }}" required>
        </div>
        <div class="mb-3">
            <label>ID Dosen Wali</label>
            <input type="number" name="id_dosen" class="form-control" value="{{ $mahasiswa['id_dosen'] }}" required>
        </div>
        <div class="mb-3">
            <label>ID Kajur</label>
            <input type="number" name="id_kajur" class="form-control" value="{{ $mahasiswa['id_kajur'] }}" required>
        </div>
        <div class="mb-3">
            <label>Nama Mahasiswa</label>
            <input type="text" name="nama_mahasiswa" class="form-control" value="{{ $mahasiswa['nama_mahasiswa'] }}" required>
        </div>
        <div class="mb-3">
            <label>Tempat Tanggal Lahir</label>
            <input type="text" name="tempat_tanggal_lahir" class="form-control" value="{{ $mahasiswa['tempat_tanggal_lahir'] }}" required>
        </div>
        <div class="mb-3">
            <label>Jenis Kelamin</label>
            <input type="text" name="jenis_kelamin" class="form-control" value="{{ $mahasiswa['jenis_kelamin'] }}" required>
        </div>
        <div class="mb-3">
            <label>Alamat</label>
            <input type="text" name="alamat" class="form-control" value="{{ $mahasiswa['alamat'] }}" required>
        </div>
        <div class="mb-3">
            <label>Agama</label>
            <input type="text" name="agama" class="form-control" value="{{ $mahasiswa['agama'] }}" required>
        </div>
        <div class="mb-3">
            <label>Angkatan</label>
            <input type="number" name="angkatan" class="form-control" value="{{ $mahasiswa['angkatan'] }}" required>
        </div>
        <div class="mb-3">
            <label>Program Studi</label>
            <input type="text" name="program_studi" class="form-control" value="{{ $mahasiswa['program_studi'] }}" required>
        </div>
        <div class="mb-3">
            <label>Semester</label>
            <input type="number" name="semester" class="form-control" value="{{ $mahasiswa['semester'] }}" required>
        </div>
        <div class="mb-3">
            <label>No HP</label>
            <input type="text" name="no_hp" class="form-control" value="{{ $mahasiswa['no_hp'] }}" required>
        </div>  
        <div class="mb-3">
            <label>Email</label>
            <input type="email" name="email" class="form-control" value="{{ $mahasiswa['email'] }}" required>
        </div>
        <button class="btn btn-primary">Update</button>
    </form>
</div>
@endsection
```

3. create.blade.php
```
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Tambah Mahasiswa</h1>
    <form method="POST" action="{{ route('mahasiswa.store') }}">
        @csrf
        <div class="mb-3">
            <label>NPM</label>
            <input type="text" name="npm" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>ID User</label>
            <input type="number" name="id_user" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>ID Dosen Wali</label>
            <input type="number" name="id_dosen" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>ID Kajur</label>
            <input type="number" name="id_kajur" class="form-control" required> 
        </div>
        <div class="mb-3">
            <label>Nama Mahasiswa</label>
            <input type="text" name="nama_mahasiswa" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Tempat Tanggal Lahir</label>
            <input type="text" name="tempat_tanggal_lahir" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Jenis Kelamin</label>
            <input type="text" name="jenis_kelamin" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Alamat</label>
            <input type="text" name="alamat" class="form-control" required>
        </div>        
        <div class="mb-3">
            <label>Agama</label>
            <input type="text" name="agama" class="form-control" required>    
        </div>  
        <div class="mb-3">
            <label>Angkatan</label>
            <input type="number" name="angkatan" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>program_studi</label>
            <input type="text" name="program_studi" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Semester</label>
            <input type="number" name="semester" class="form-control" required>
        </div>

        <div class="mb-3">
            <label>No HP</label>
            <input type="text" name="no_hp" class="form-control" required>
        </div>  
        <div class="mb-3">
            <label>Email</label>
            <input type="email" name="email" class="form-control" required>
        </div>
        <button class="btn btn-success">Simpan</button>
    </form>
</div>
@endsection
```

- dosen_wali
1. index.blade.php
```
 @extends('layouts.app') 

 @section('content') 
<div class="container">
    <h1>Daftar Dosen Wali</h1>
    <a href="{{ route('dosen_wali.create') }}" class="btn btn-primary mb-3">Tambah Dosen</a>

    @if (session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    <table class="table table-bordered">
        <thead>
            <tr>
                <th>ID</th>
                <th>Nama</th>
                <th>NIDN</th>
                <th>ID User</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($dosen_wali as $dosen)
            <tr>
                <td>{{ $dosen['id_dosen'] }}</td>
                <td>{{ $dosen['nama_dosen'] }}</td>
                <td>{{ $dosen['nidn'] }}</td>
                <td>{{ $dosen['id_user'] }}</td>
                <td>
                    <a href="{{ route('dosen_wali.edit', $dosen['id_dosen']) }}" class="btn btn-sm btn-warning">Edit</a>
                    <form method="POST" action="{{ route('dosen_wali.destroy', $dosen['id_dosen']) }}" style="display:inline;">
                        @csrf
                        @method('DELETE')
                        <button onclick="return confirm('Yakin ingin hapus?')" class="btn btn-sm btn-danger">Hapus</button>
                    </form>
                </td>
            </tr>
            @endforeach
        </tbody>
    </table>
</div>
 @endsection 
```

2. edit.php
```
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Edit Dosen</h1>
    <form method="POST" action="{{ route('dosen_wali.update', $dosen['id_dosen']) }}">
        @csrf
        @method('PUT')
        <div class="mb-3">
            <label>Nama Dosen</label>
            <input type="text" name="nama_dosen" class="form-control" value="{{ $dosen['nama_dosen'] }}" required>
        </div>
        <div class="mb-3">
            <label>NIDN</label>
            <input type="text" name="nidn" class="form-control" value="{{ $dosen['nidn'] }}" required>
        </div>
        <div class="mb-3">
            <label>ID User</label>
            <input type="number" name="id_user" class="form-control" value="{{ $dosen['id_user'] }}" required>
        </div>
        <button class="btn btn-primary">Update</button>
    </form>
</div>
@endsection
```

3. create.blade.php
```
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Tambah Dosen</h1>
    <form method="POST" action="{{ route('dosen_wali.store') }}">
        @csrf
        <div class="mb-3">
            <label>Nama Dosen</label>
            <input type="text" name="nama_dosen" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>NIDN</label>
            <input type="text" name="nidn" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>ID User</label>
            <input type="number" name="id_user" class="form-control" required>
        </div>
        <button class="btn btn-success">Simpan</button>
    </form>
</div>
@endsection
```

3. Routes
- web.php
```
<?php

use Illuminate\Support\Facades\Route;

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider and all of them will
| be assigned to the "web" middleware group. Make something great!
|
*/

Route::get('/', function () {
    return view('welcome');
});
Route::resource('dosen_wali', \App\Http\Controllers\DosenWaliController::class);
Route::resource('mahasiswa', \App\Http\Controllers\MahasiswaController::class);
```

- Layouts app.blade.php
```
<!DOCTYPE html>
<html>
<head>
    <title>Aplikasi Mahasiswa</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body>
    <div class="container mt-4">
        @yield('content')
    </div>
</body>
</html>
```
