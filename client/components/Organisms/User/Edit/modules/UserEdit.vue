<template>
    <div class="icon_container">
        <div class="icon">
            <!-- icon -->
            <img :src="userIconUrl" />
            <input class="image_input" type="file" @change="getImageFile" />
        </div>
        <div class="name">
            <!-- name -->
            <div class="name_inner">
                <app-input v-model="userModel.name" class="input" />
                <div>さん</div>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import { UserModel } from 'chillnn-training-abr'
import { Component, Prop, Vue } from 'nuxt-property-decorator'
// component
import AppInput from '@/components/Atom/AppInput.vue'
import { getImage, HTMLInputEvent } from '~/util/imageUtil'
import { AsyncLoadingAndErrorHandle } from '~/util/decorator/baseDecorator'

@Component({
    components: {
        AppInput,
    },
})
export default class UserIcon extends Vue {
    @Prop({ required: true }) userModel!: UserModel

    get userName() {
        return this.userModel.name
    }

    get userIconUrl() {
        return this.userModel.userIcon
    }

    @AsyncLoadingAndErrorHandle()
    public async getImageFile(e: HTMLInputEvent) {
        const image = await getImage(e) // resizeしている
        await this.userModel.setIcon(image.file)
    }
}
</script>
<style lang="stylus" scoped>
.icon_container {
    .icon {
        text-align: center;
        position: relative;

        img {
            box-shadow: 0 0 5px $shadowColor;
            border-radius: 10000px;
            width: 200px;
            height: 200px;
            object-fit: cover;

            @media only screen and (max-width: $spSize) {
                width: 100px;
                height: 100px;
            }
        }

        .image_input {
            position: absolute;
            display: inline-block;
            top: 0;
            left: calc(50% - 100px);
            width: 200px;
            height: 200px;
            border-width: inherit;
            border-width: inherit;
            cursor: pointer;
            opacity: 0;

            @media only screen and (max-width: $spSize) {
                width: 100px;
                height: 100px;
                left: calc(50% - 50px);
            }
        }
    }

    .name {
        padding-top: 10px;
        text-align: center;
        font-size: 14px;

        .name_inner {
            display: flex;
            justify-content: center;
            align-items: center;

            .input {
                box-sizing: border-box;
                width: 70%;
                padding-right: 5px;
            }
        }
    }
}
</style>
