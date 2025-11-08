create table
   user(
      user_id char(36) primary key,
      first_name varchar(50) not null,
      last_name varchar(50) not null,
      email varchar(255) not null unique,
      password_hash varchar(255) not null,
      phone_number varchar(20) null,
      role enum ('guest', 'host', 'admin') not null,
      created_at timestamp default current_timestamp,
      index idx_user_email (email)
   );

create table
   property (
      property_id char(36) primary key,
      host_id char(36) not null,
      foreign key (host_id) references user(user_id),
      name varchar(100) not null,
      description text not null,
      location varchar(255) not null,
      pricepernight decimal(10, 2) not null,
      created_at timestamp default current_timestamp,
      updated_at timestamp default current_timestamp on update current_timestamp
   );

create table
   booking (
      booking_id char(36) primary key,
      property_id char(36) not null,
      foreign key (property_id) references property (property_id),
      user_id char(36) not null,
      foreign key (user_id) references user(user_id),
      status enum ('pending', 'confirmed', 'canceled') not null,
      total_price decimal(10, 2) not null,
      start_date date not null,
      end_date date not null,
      created_at timestamp default current_timestamp
   );

create table
   payment (
      payment_id char(36) primary key,
      booking_id char(36) not null,
      foreign key (booking_id) references booking (booking_id),
      amount decimal(10, 2) not null,
      payment_date timestamp default current_timestamp,
      payment_method enum ('credit_card', 'paypal', 'stripe') not null
   );

create table
   review (
      review_id char(36) primary key,
      property_id char(36) not null,
      foreign key (property_id) references property (property_id),
      user_id char(36) not null,
      foreign key (user_id) references user(user_id),
      rating int check (
         rating >= 1
         and rating <= 5
      ) not null,
      comment text not null,
      created_at timestamp default current_timestamp
   );

create table
   message (
      message_id char(36) primary key,
      sender_id char(36) not null,
      foreign key (sender_id) references user(user_id),
      recipient_id char(36) not null,
      foreign key (recipient_id) references user(user_id),
      message_body text not null,
      sent_at timestamp default current_timestamp
   );

create index idx_property_id_property on property (property_id);

create index idx_property_id_booking on booking (property_id);

create index idx_booking_id_booking on booking (booking_id);

create index idx_booking_id_payment on payment (booking_id);
