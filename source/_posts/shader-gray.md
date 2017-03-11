---
title: shader置灰
date: 2016-12-05 22:13:18
tags:
categories: shader
---
## vsh
        attribute vec4 a_position; 
        attribute vec2 a_texCoord;
        attribute vec4 a_color;

        varying vec4 v_fragmentColor;
        varying vec2 v_texCoord;
        void main()
        {
            gl_Position = CC_MVPMatrix * a_position;  //这里CC_MVPMatrix 会使得位置偏移
            v_fragmentColor = a_color;
            v_texCoord = a_texCoord;
        }


## fsh
        varying vec4 v_fragmentColor;
        varying vec2 v_texCoord;
        uniform sampler2D CC_Texture4;
        void main()
        {
        vec4 v_orColor = v_fragmentColor * texture2D(CC_Texture4, v_texCoord);
        float gray = dot(v_orColor.rgb, vec3(0.299, 0.587, 0.114));
        gl_FragColor = vec4(gray, gray, gray, v_orColor.a);
        }
