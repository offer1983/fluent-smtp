<template>
    <div>
        <h3 class="fs_config_title">Postmark API Settings</h3>
        <el-radio-group size="mini" v-model="connection.key_store">
            <el-radio-button label="db">Store API Keys in DB</el-radio-button>
            <el-radio-button label="wp_config">Store API Keys in Config File</el-radio-button>
        </el-radio-group>

        <el-form-item v-if="connection.key_store == 'db'">
            <label for="postmark-key">
                API Key
            </label>

            <InputPassword
                id="postmark-key"
                v-model="connection.api_key"
            />
            
            <error :error="errors.get('api_key')" />

        </el-form-item>

        <div class="fss_condesnippet_wrapper" v-else-if="connection.key_store == 'wp_config'">
            <el-form-item>
                <label>Simply copy the following snippet and replace the stars with the corresponding credential. Then simply paste to wp-config.php file of your WordPress installation</label>
                <div class="code_snippet">
                    <textarea readonly style="width: 100%;">define( 'FLUENTMAIL_POSTMARK_API_KEY', '********************' );</textarea>
                </div>
                <error :error="errors.get('api_key')" />
            </el-form-item>
        </div>

        <span class="small-help-text" style="display:block;margin-top:-10px">
            Follow this link to get an API Key from Postmark (Your API key is in the API Tokens tab of your):
            <a target="_blank" href="https://account.postmarkapp.com/servers">Postmark Server.</a>
        </span>

        <el-row class="fsmtp_compact" :gutter="30">
            <el-col :span="12">
                <el-form-item label="Track Opens">
                    <el-checkbox
                        true-label="yes"
                        false-label="no"
                        v-model="connection.track_opens"
                    >
                        Enable email opens tracking on postmark (For HTML Emails only).
                        <el-tooltip effect="dark" placement="top-start">
                            <div slot="content">
                                If you enable this then open tracking header will be added to the email for postmark.
                            </div>
                            <i class="el-icon-info"></i>
                        </el-tooltip>
                    </el-checkbox>
                </el-form-item>
                <el-form-item label="Message Stream">
                    <el-input type="text" size="small" v-model="connection.message_stream" />
                </el-form-item>
            </el-col>
            <el-col :span="12">
                <el-form-item label="Track Links">
                    <el-checkbox
                        true-label="yes"
                        false-label="no"
                        v-model="connection.track_links"
                    >
                        Enable link tracking on postmark (For HTML Emails only).
                        <el-tooltip effect="dark" placement="top-start">
                            <div slot="content">
                                If you enable this then link tracking header will be added to the email for postmark.
                            </div>
                            <i class="el-icon-info"></i>
                        </el-tooltip>
                    </el-checkbox>
                </el-form-item>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    import InputPassword from '@/Pieces/InputPassword';
    import Error from '@/Pieces/Error';

    export default {
        name: 'PostMark',
        props: ['connection', 'errors'],
        components: {
            InputPassword,
            Error
        },
        'connection.key_store'(value) {
            if (value === 'wp_config') {
                this.connection.api_key = '';
            }
        },
        data() {
            return {
                // ...
            };
        }
    };
</script>
