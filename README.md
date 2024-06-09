<?php
// Include file cấu hình ban đầu của `Twig`
require_once __DIR__.'/../bootstrap.php';

// Truy vấn database để lấy danh sách
// 1. Include file cấu hình kết nối đến database, khởi tạo kết nối $conn
include_once(__DIR__.'/../dbconnect.php');

// 2. Chuẩn bị câu truy vấn $sql
// HEREDOC
$sqlDanhSachSanPham = <<<EOT
    SELECT sp.sp_ma, sp.sp_ten, sp.sp_gia, sp.sp_giacu, sp.sp_mota_ngan, sp.sp_soluong, lsp.lsp_ten, MAX(hsp.hsp_tentaptin) AS hsp_tentaptin
    FROM `sanpham` sp
    JOIN `loaisanpham` lsp ON sp.lsp_ma = lsp.lsp_ma
    LEFT JOIN `hinhsanpham` hsp ON sp.sp_ma = hsp.sp_ma
    GROUP BY sp.sp_ma, sp.sp_ten, sp.sp_gia, sp.sp_giacu, sp.sp_mota_ngan, sp.sp_soluong, lsp.lsp_ten
EOT;

// 3. Thực thi câu truy vấn SQL để lấy về dữ liệu
$result = mysqli_query($conn, $sqlDanhSachSanPham);

// 4. Khi thực thi các truy vấn dạng SELECT, dữ liệu lấy về cần phải phân tích để sử dụng
// Thông thường, chúng ta sẽ sử dụng vòng lặp while để duyệt danh sách các dòng dữ liệu được SELECT
// Ta sẽ tạo 1 mảng array để chứa các dữ liệu được trả về
$dataDanhSachSanPham = [];
while($row = mysqli_fetch_array($result, MYSQLI_ASSOC))
{
    $dataDanhSachSanPham[] = array(
        'sp_ma' => $row['sp_ma'],
        'sp_ten' => $row['sp_ten'],
        'sp_gia' => number_format($row['sp_gia'], 2, ".", ",") . ' vnđ',
        'sp_giacu' => number_format($row['sp_giacu'], 2, ".", ","),
        'sp_mota_ngan' => $row['sp_mota_ngan'],
        'sp_soluong' => $row['sp_soluong'],
        'lsp_ten' => $row['lsp_ten'],
        'hsp_tentaptin' => $row['hsp_tentaptin'],
    );
}

//var_dump($dataDanhSachSanPham);die;
print_r($dataDanhSachSanPham);die;

// Yêu cầu `Twig` vẽ giao diện được viết trong file `frontend/pages/home.html.twig`
// với dữ liệu truyền vào file giao diện được đặt tên
// echo $twig->render('frontend/pages/home.html.twig', [
//     'danhsachsanpham' => $dataDanhSachSanPham
// ]);
?>
GIAO DIỆN TEMPLETE
{# Kế thừa layout frontend #}
{% extends "frontend/layouts/layout.html.twig" %}

{# Nội dung trong block title #}
{% block title %}
Trang chủ
{% endblock %}
{# End Nội dung trong block title #}

{# Nội dung trong block content #}
{% block content %}
<!-- Carousel - Slider -->
<div id="myCarousel" class="carousel slide" data-ride="carousel">
    <ol class="carousel-indicators">
        <li data-target="#myCarousel" data-slide-to="0" class="active"></li>
        <li data-target="#myCarousel" data-slide-to="1"></li>
        <li data-target="#myCarousel" data-slide-to="2"></li>
    </ol>
    <div class="carousel-inner">
        <div class="carousel-item active">
            <img src="/assets/frontend/img/slider-1.jpg" />
            <div class="container">
                <div class="carousel-caption text-left">
                    <h1>Cơ sở lưu trú - Nơi mua sắm tuyệt vời</h1>

                </div>
            </div>
        </div>
        <div class="carousel-item">
            <img src="/assets/frontend/img/slider-2.jpg" />
            <div class="container">
                <div class="carousel-caption">
                    <h1>Hàng triệu sản phẩm - Lựa chọn mỏi tay</h1>

                </div>
            </div>
        </div>
        <div class="carousel-item">
            <img src="/assets/frontend/img/slider-3.jpg" />
            <div class="container">
                <div class="carousel-caption text-right">
                    <h1>Chất lượng là Hàng đầu.</h1>

                </div>
            </div>
        </div>
    </div>
    <a class="carousel-control-prev" href="#myCarousel" role="button" data-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="sr-only">Previous</span>
    </a>
    <a class="carousel-control-next" href="#myCarousel" role="button" data-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="sr-only">Next</span>
    </a>
</div>

<!-- Tính năng Marketing -->
<div class="container marketing">
    <!-- Three columns of text below the carousel -->
    <div class="row">
        <div class="col-lg-4">
            <img class="bd-placeholder-img rounded-circle" width="140" height="140"
                src="/assets/frontend/img/icon-1.png" />
            <h2>Đặt hàng</h2>
            <p>Chọn sản phẩm bạn yêu thích, và Đặt hàng.</p>
        </div><!-- /.col-lg-4 -->
        <div class="col-lg-4">
            <img class="bd-placeholder-img rounded-circle" width="140" height="140"
                src="/assets/frontend/img/icon-2.png" />
            <h2>Tạo đơn hàng</h2>
            <p>Theo dõi đơn hàng của bạn.</p>
        </div><!-- /.col-lg-4 -->
        <div class="col-lg-4">
            <img class="bd-placeholder-img rounded-circle" width="140" height="140"
                src="/assets/frontend/img/icon-3.png" />
            <h2>Giao hàng</h2>
            <p>Giao hàng tận nơi.</p>
        </div><!-- /.col-lg-4 -->
    </div><!-- /.row -->


    <!-- START THE FEATURETTES -->

    <hr class="featurette-divider">

    <div class="row featurette">
        <div class="col-md-7">
            <h2 class="featurette-heading">Đặt hàng, Tạo đơn hàng, Giao hàng <span class="text-muted">Nhanh chóng</span>
            </h2>
            <p class="lead">Nơi mua sắm tuyệt vời cho mọi lứa tuổi.</p>
        </div>
        <div class="col-md-5">
            <img src="/assets/frontend/img/marketing-1.png" />
        </div>
    </div>

    <hr class="featurette-divider">

    <div class="row featurette">
        <div class="col-md-7 order-md-2">
            <h2 class="featurette-heading">Báo cáo Doanh thu tuyệt vời <span class="text-muted">Theo dõi đơn hàng của
                    bạn.</span></h2>
            <p class="lead">Hệ thống theo dõi đơn hàng chi tiết, thông tin mọi lúc mọi nơi.</p>
        </div>
        <div class="col-md-5 order-md-1">
            <img src="/assets/frontend/img/marketing-2.png" />
        </div>
    </div>

    <hr class="featurette-divider">

    <!-- /END THE FEATURETTES -->

</div>

<!-- Danh sách sản phẩm -->
<section class="jumbotron text-center">
    <div class="container">
        <h1 class="jumbotron-heading">Danh sách Sản phẩm</h1>
        <p class="lead text-muted">Các sản phẩm với chất lượng, uy tín, cam kết từ nhà Sản xuất, phân phối và bảo hành
            chính hãng.</p>
    </div>
</section>

<!-- Giải thuật duyệt và render Danh sách sản phẩm theo dòng, cột của Bootstrap -->
{% set counter = 0 %}
{% set group = 3 %}
{% set limit = danhsachsanpham|length %}
{% set col_class_name = (12 / group) %}
<div class="danhsachsanpham py-5 bg-light">
    <div class="container">
        <div class="row">
            {% for sanpham in danhsachsanpham %}
            <div class="col-md-{{ col_class_name }}">
                <div class="card mb-4 shadow-sm">
                    {% if sanpham.hsp_tentaptin %}
                    <a href="/frontend/sanpham/chitiet.php?sp_ma={{ sanpham.sp_ma }}">
                        <img class="bd-placeholder-img card-img-top" width="100%" height="350"
                            src="/assets/uploads/{{ sanpham.hsp_tentaptin }}" />
                    </a>
                    {% else %}
                    <a href="/frontend/sanpham/chitiet.php?sp_ma={{ sanpham.sp_ma }}">
                        <img class="bd-placeholder-img card-img-top" width="100%" height="350"
                            src="/assets/shared/img/default-image_600.png" />
                    </a>
                    {% endif %}
                    <div class="card-body">
                        <a href="/frontend/sanpham/chitiet.php?sp_ma={{ sanpham.sp_ma }}">
                            <h5>{{ sanpham.sp_ten }}</h5>
                        </a>
                        <p class="card-text">{{ sanpham.sp_mota_ngan }}</p>
                        <div class="d-flex justify-content-between align-items-center">
                            <div class="btn-group">
                                <a class="btn btn-sm btn-outline-secondary" href="/frontend/sanpham/chitiet.php?sp_ma={{ sanpham.sp_ma }}">Xem chi tiết</a>
                            </div>
                            <small class="text-muted text-right">
                                <s>{{ sanpham.sp_giacu }}</s>
                                <b>{{ sanpham.sp_gia }}</b>
                            </small>
                        </div>
                    </div>
                </div>
            </div>
            {% set counter = counter + 1 %}
            {% if (counter % group == 0 and counter < limit) %}
            </div><div class="row">
            {% endif %}

            {% endfor %}
        </div>
    </div>
</div>


{% endblock %}
{# End Nội dung trong block content #}
