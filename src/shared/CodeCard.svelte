<script>
    import dayjs from 'dayjs';
    export let code;
   
</script>

<div class="card card-shadow">
    <div class="card-content">
        <h2 class="is-size-5 has-text-weight-semibold">{code.title}</h2>
        <h6 class="is-size-7 has-text-grey">            
            {code.creator_username}•{dayjs(code.created).format('h:mmA - DD/MM/YYYY')}
        </h6>
        <p>
            {code.body_plain}
        </p>
    </div>
</div>