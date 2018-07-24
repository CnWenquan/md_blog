---
title: smartWizard向导插件的使用
url: 535.html
id: 535
categories:
  - js/jquery/ajax
date: 2017-06-26 15:40:46
tags:
---

效果图：

![image.png](/ueditor/php/upload/image/20170626/1498462651973411.png "1498462651973411.png")

首先，引入jquery.smartWizard-2.0.js  

  

html元素：

<div id="wizard" class="swMain">
                                <ul>
                                    <li><a href="#step-1"><span class="stepNumber">1</span><span class="stepDesc">第一步<br /><small>选择版本</small></span></a></li>
                                    <li><a href="#step-2"><span class="stepNumber">2</span><span class="stepDesc">第二步<br /><small>筛选影院、设备</small></span></a></li>
                                    <li><a href="#step-3"><span class="stepNumber">3</span><span class="stepDesc">第三步<br /><small>展示结果</small></span></a></li>
                                </ul>
                                <div id="step-1">	
                                    <h2 class="StepTitle">选择版本</h2>
                                    <table class="table table-striped">
                                        <thead>
                                            <tr>
                                                <th></th>
                                                <th>影像类型</th>
                                                <th>画面类型</th>
                                                <th>声音制式</th>
                                                <th>语种</th>
                                                <th>影片格式</th>
                                                <th></th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <?PHP foreach ($version as $key => $v) { ?>
                                                <tr>
                                                    <td><?PHP echo ($key + 1); ?></td>
                                                    <td><?PHP echo $options\['image\_type'\]\[$v\['image\_type'\]\]; ?></td>
                                                    <td><span class="pie"><?PHP echo $options\['frame\_type'\]\[$v\['frame\_type'\]\]; ?></span></td>
                                                    <td><?PHP echo $options\['sound\_type'\]\[$v\['sound\_type'\]\]; ?></td>
                                                    <td><?PHP echo $options\['language\_type'\]\[$v\['language\_type'\]\]; ?></td>
                                                    <td><?PHP echo $options\['film\_format'\]\[$v\['film\_format'\]\]; ?></td>
                                                    <td><input type="checkbox" class="i-checks versions\_check\_status" name="version_type" value="<?PHP echo $v\['id'\]; ?>"></td>
                                                </tr>
                                            <?PHP } ?>
                                        </tbody>
                                    </table>
                                </div>
                                <div id="step-2">
                                    <h2 class="StepTitle">Step 2 Content</h2>	
                                    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
                                    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
                                    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p> 
                                    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>          
                                </div>                      
                                <div id="step-3">
                                    <h2 class="StepTitle">Step 3 Content</h2>	
                                    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit,sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
                                    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>               				          
                                </div>
                            </div>

js代码：  

$(document).ready(function () {
//                $('#wizard').smartWizard();
                $('#wizard').smartWizard({
                    transitionEffect: 'slideleft',
                    onLeaveStep: leaveAStepCallback,
                    onFinish: onFinishCallback,
                    enableFinishButton: true
                });

                function leaveAStepCallback(obj) {
                    var step_num = obj.attr('rel');
                    return validateSteps(step_num);
                }

                function onFinishCallback() {
                    if (validateAllSteps()) {
                        /*
                         * ajax请求
                         */
                        jQuery('form').submit();
                    }
                }

                $(".i-checks").iCheck({
                    checkboxClass: "icheckbox_square-green",
                    radioClass: "iradio_square-green"
                });

            });
            
            function validateAllSteps() {
                var isStepValid = true;

                if (validateStep1() == false) {
                    isStepValid = false;
                    $('#wizard').smartWizard('setError', {stepnum: 1, iserror: true});
                } else {
                    $('#wizard').smartWizard('setError', {stepnum: 1, iserror: false});
                }
                
                if (validateStep2() == false) {
                    isStepValid = false;
                    $('#wizard').smartWizard('setError', {stepnum: 2, iserror: true});
                } else {
                    $('#wizard').smartWizard('setError', {stepnum: 2, iserror: false});
                }

                if (validateStep3() == false) {
                    isStepValid = false;
                    $('#wizard').smartWizard('setError', {stepnum: 3, iserror: true});
                } else {
                    $('#wizard').smartWizard('setError', {stepnum: 3, iserror: false});
                }

                if (!isStepValid) {
                    $('#wizard').smartWizard('showMessage', '请修正有错误的步骤，然后继续');
                }

                return isStepValid;
            }

            function validateSteps(step) {
                var isStepValid = true;
                // validate step 1 
                if (step == 1) {
                    if (validateStep1() == false) {
                        isStepValid = false;
                        $('#wizard').smartWizard('showMessage', '请选择故障注入对象（勾选上边的树），然后点击下一步.');
                        $('#wizard').smartWizard('setError', {stepnum: step, iserror: true});
                    } else {
                        $('#wizard').smartWizard('setError', {stepnum: step, iserror: false});
                    }
                }
                
                // validate step 2 
                if (step == 2) {
                    if (validateStep2() == false) {
                        isStepValid = false;
                        $('#wizard').smartWizard('showMessage', '请选择故障注入对象（勾选上边的树），然后点击下一步.');
                        $('#wizard').smartWizard('setError', {stepnum: step, iserror: true});
                    } else {
                        $('#wizard').smartWizard('setError', {stepnum: step, iserror: false});
                    }
                }

                // validate step3 
                if (step == 3) {
                    if (validateStep3() == false) {
                        isStepValid = false;
                        $('#wizard').smartWizard('showMessage', '请将故障参数配置完整，然后点击下一步.');
                        $('#wizard').smartWizard('setError', {stepnum: step, iserror: true});
                    } else {
                        $('#wizard').smartWizard('setError', {stepnum: step, iserror: false});
                    }
                }

                return isStepValid;
            }

            function validateStep1() {
                var isValid = true;
                // 这里写自己的校验代码 
                return isValid;
            }
            
            function validateStep2() {
                var isValid = false;
                // 这里写自己的校验代码 
                return isValid;
            }

            function validateStep3() {
                var isValid = false;
                // 这里写自己的校验代码 
                return isValid;
            }