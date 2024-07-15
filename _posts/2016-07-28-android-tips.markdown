---
layout: post
title: 一个Class搞定提示框，懒人必备
categories: [安卓]
tags: [安卓]
date: 2016-07-28 15:32:24.000000000 +09:00
---

#### 前言
做新项目时，要用到以前的代码，有许多关联的资源文件要拷贝，但是又属于比较懒的那种人。就将布局文件用代码生成，省得拷贝。

#### 具体代码
	package com.alan.dialogdemo;
	
	import android.annotation.SuppressLint;
	import android.app.Dialog;
	import android.content.Context;
	import android.graphics.Color;
	import android.text.TextUtils;
	import android.view.Gravity;
	import android.view.View;
	import android.view.Window;
	import android.widget.Button;
	import android.widget.LinearLayout;
	import android.widget.LinearLayout.LayoutParams;
	import android.widget.TextView;
	
	/**
	 * @author Alan
	 * @date 创建时间：2016年7月25日 下午2:48:05
	 * @version 0.5
	 * @Description  通用提示框
	 * @return  
	 * */
	@SuppressLint("NewApi")
	public class CommonDialog extends Dialog{
		
		public CommonDialog(Context context){
			super(context);
		}
		
		public interface OnCommonDialogClickListener{
			public void confirm();
			public void cancel();
		}
		
	    public static class Builder{
	    	private Context mContext;
	    	/** 标题*/
	    	private TextView tvTitle;
	    	/** 提示内容*/
	    	private TextView tvContent;
	    	/** 确定按钮*/
	    	private Button btnConfire;
	    	/** 取消按钮*/
	    	private Button btnCancel;
	    	/** 提示标题图片资源*/
	    	private String mTitleStr;
	    	/** 提示内容文本*/
	    	private String mContentStr = "";
	    	/** 确定按钮文本*/
	    	private String mConfireStr = "";
	    	/** 取消按钮文本*/
	    	private String mCancelStr = "";
	    	/** 点击外面是否取消,默认不能取消*/
	    	private boolean mCancelableFlag = false;
	    	/** 操作回调*/
	    	private static OnCommonDialogClickListener mListener;
	    	
	    	public Builder(Context context){
	    		this.mContext = context;
	    		this.mConfireStr = "确定";
	    		this.mCancelStr = "取消";
	    	}
	    	
	    	public Builder(Context context,String title,String content,OnCommonDialogClickListener listener){
	    		
	    	}
	    	
	    	public CommonDialog.Builder setOnCommonDialogClick(OnCommonDialogClickListener listener){
	    		mListener = listener;
	    		return this;
	    	}
	    	
	    	public CommonDialog.Builder setTitle(String title) {
	    		mTitleStr = title;
				return this;
			}
	    	
	    	public CommonDialog.Builder setContent(String content) {
	    		mContentStr = content;
				return this;
			}
	    	
	    	public CommonDialog.Builder setCancelable(boolean flag){
	    		mCancelableFlag = flag;
				return this;
	    	}  
	    	
	    	public CommonDialog create(){
	    		
	    		final CommonDialog dialog = new CommonDialog(mContext);
	    		dialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
	    		dialog.getWindow().setBackgroundDrawableResource(android.R.color.transparent);
	    		dialog.setCancelable(mCancelableFlag);
	    		// 根布局
	    		LinearLayout mRootView = new LinearLayout(mContext);
	    		mRootView.setOrientation(LinearLayout.VERTICAL);
	    		mRootView.setLayoutParams(new LinearLayout.LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.MATCH_PARENT));
	    		mRootView.setBackgroundColor(Color.WHITE);
	    		// 标题布局
	    		LinearLayout mTitleLayout = new LinearLayout(mContext);
	    		// 标题布局参数
	    		LinearLayout.LayoutParams mTitleLayoutParams = new LinearLayout.LayoutParams(LayoutParams.MATCH_PARENT,dipTopx(mContext, 50));
	    		mTitleLayoutParams.gravity = Gravity.CENTER;
	    		mTitleLayout.setLayoutParams(mTitleLayoutParams);
	    		mTitleLayout.setBackgroundColor(Color.DKGRAY);
	    		mTitleLayout.setGravity(Gravity.CENTER);
	    		// 标题文本
	    		TextView mTitle = new TextView(mContext);
	    		LinearLayout.LayoutParams mTitleParams = new LinearLayout.LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.MATCH_PARENT);
	    		mTitleParams.gravity = Gravity.CENTER;
	    		mTitle.setLayoutParams(mTitleParams);
	    		mTitle.setPadding(dipTopx(mContext, 10), dipTopx(mContext, 10), dipTopx(mContext, 10), dipTopx(mContext, 10));
	    		if(TextUtils.isEmpty(mTitleStr)){
	    			throw new RuntimeException("请设置标题 setTitle");
	    		}
	    		mTitle.setText(mTitleStr);
	    		mTitle.setTextSize(25);
	    		mTitle.setGravity(Gravity.CENTER);
	    		mTitle.setTextColor(Color.WHITE);
	    		mTitleLayout.addView(mTitle);
	    		mRootView.addView(mTitleLayout);
	    		// 提示内容
	    		TextView mContent = new TextView(mContext);
	    		LinearLayout.LayoutParams mContentParams = new LinearLayout.LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT);
	    		mContentParams.setMargins(0, dipTopx(mContext, 40), 0, dipTopx(mContext, 40));
	    		mContentParams.gravity = Gravity.CENTER;
	    		mContent.setLayoutParams(mContentParams);
	    		mContent.setGravity(Gravity.CENTER);
	    		if(TextUtils.isEmpty(mContentStr)){
	    			throw new RuntimeException("请设置提示内容 setContent");
	    		}
	    		mContent.setText(mContentStr);
	    		mContent.setTextColor(Color.BLACK);
	    		mContent.setTextSize(18);
	    		mRootView.addView(mContent);
	    		// 按钮布局
	    		LinearLayout mBtnLayout = new LinearLayout(mContext);
	    		mBtnLayout.setGravity(Gravity.CENTER_HORIZONTAL);
	    		LinearLayout.LayoutParams mBtnLayoutParams = new LinearLayout.LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT);
	    		mBtnLayoutParams.setMargins(0, 0, 0, dipTopx(mContext, 10));
	    		mBtnLayout.setLayoutParams(mBtnLayoutParams);
	    		// 取消按钮
	    		Button mCancelBtn = new Button(mContext);
	    		LinearLayout.LayoutParams mCancelBtnParams = new LinearLayout.LayoutParams(dipTopx(mContext, 150), LayoutParams.WRAP_CONTENT);
	    		mCancelBtnParams.leftMargin = (dipTopx(mContext, 10));
	    		mCancelBtn.setLayoutParams(mCancelBtnParams);
	    		mCancelBtn.setText(mCancelStr);
	    		mCancelBtn.setPadding(dipTopx(mContext, 10), dipTopx(mContext, 5), dipTopx(mContext, 10), dipTopx(mContext, 5));
	   
	    		// 取消按钮点击事件
	    		mCancelBtn.setOnClickListener(new View.OnClickListener() {
					
					@Override
					public void onClick(View v) {
						
						if(mListener != null){
							mListener.cancel();
							dialog.dismiss();
						}
					}
				});
	    		// 确定按钮
	    		Button mConfireBtn = new Button(mContext);
	    		LinearLayout.LayoutParams mConfireBtnParams = new LinearLayout.LayoutParams(dipTopx(mContext, 150), LayoutParams.WRAP_CONTENT);
	    		mConfireBtnParams.rightMargin = (dipTopx(mContext, 10));
	    		mConfireBtn.setLayoutParams(mConfireBtnParams);
	    		mConfireBtn.setText(mConfireStr);
	    		mConfireBtn.setPadding(dipTopx(mContext, 10), dipTopx(mContext, 5), dipTopx(mContext, 10), dipTopx(mContext, 5));
	
	    		// 确定按钮点击事件
	    		mConfireBtn.setOnClickListener(new View.OnClickListener() {
					
					@Override
					public void onClick(View v) {
						if(mListener != null){
							mListener.confirm();
							dialog.dismiss();
						}
					}
				});
	    		
	    		mBtnLayout.addView(mCancelBtn);
	    		mBtnLayout.addView(mConfireBtn);
	    		mRootView.addView(mBtnLayout);
	    		
	    		dialog.setContentView(mRootView);
	    		
	    		return dialog;
	    	}
	    	
	    	/**  
	         * 根据手机的分辨率从 dp 的单位 转成为 px(像素)  
	         */    
	        public  int dipTopx(Context context, float dpValue) {    
	            final float scale = context.getResources().getDisplayMetrics().density;    
	            return (int) (dpValue * scale + 0.5f);    
	        }    
	         
	        /**  
	         * 根据手机的分辨率从 px(像素) 的单位 转成为 dp  
	         */   
	        public int pxTodip(Context context, float pxValue) {    
	            final float scale = context.getResources().getDisplayMetrics().density;    
	            return (int) (pxValue / scale + 0.5f);    
	        }  
				
		}
		
	}


#### 使用代码
	new CommonDialog.Builder(MainActivity.this)
						.setTitle("支付未完成")
						.setContent("你想要取消这次支付吗?")
						.setOnCommonDialogClick(new OnCommonDialogClickListener() {
							
							@Override
							public void confirm() {
								Toast.makeText(MainActivity.this, "你点击了确定", Toast.LENGTH_SHORT).show();
							}
							
							@Override
							public void cancel() {
								Toast.makeText(MainActivity.this, "你点击了取消", Toast.LENGTH_SHORT).show();
							}
						})
						.create()
						.show();


#### 最后
根据自己项目的需求更改样式，自行扩展
