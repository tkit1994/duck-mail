<script lang="ts">
    const LoginLinkApi = "/api/auth/loginlink";
    const LoginApi = "/api/auth/login";
    let token: string = "";
    let username: string = "";
    let passphrase: string = "";

    const sendMail = async () => {
        let query = new URLSearchParams({
            user: username,
        });
        await fetch(LoginLinkApi + "?" + query);
    };

    const login = async () => {
        let query = new URLSearchParams({
            otp: passphrase.trim(),
            user: username,
        });
        let res = await fetch(LoginApi + "?" + query);
        let res_json = await res.json();
        token = res_json.token;
    };

    const copyToken = async () => {
        await navigator.clipboard.writeText(token);
        alert("Token copied!");
    };
</script>

<div class="container">
    <div class="input-group">
        <input
            type="text"
            class="form-control"
            placeholder="Username"
            bind:value={username}
        />
        <span class="input-group-text">@duck.com</span>
        <div class="input-group-append">
            <button class="btn btn-outline-secondary" on:click={sendMail}>Mail</button>
        </div>
    </div>
    <div class="input-group">
        <input
            type="text"
            class="form-control"
            placeholder="Passphrase"
            bind:value={passphrase}
        />
        <div class="input-group-append">
            <button class="btn btn-outline-secondary" on:click={login}>Login</button>
        </div>
    </div>

    <button
        class="btn form-control btn-outline-primary"
        on:click={copyToken}
        disabled={token.length == 0}>Copy Token</button
    >
    <p>{token}</p>
</div>

<style>
    p {
        text-align: center;
    }
</style>
