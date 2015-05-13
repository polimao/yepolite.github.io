---
layout: post
title:  "PHP代码高亮测试"
date:   2015-05-13 08:33:06
---


```
    <?php

    class SalesCustomer extends \Precedent {

        protected $fillable = [];

        static public $actionStatusDesc = ['新客户', '已预约到店', '已到店', '已试驾', '已提车'];

        const IS_LOWEST_QUOTE = 1;          // 最低价

        const READ_LOWEST_QUOTE = 2;        // 已读的最低价

        public function actionStatusDesc()
        {
            return self::$actionStatusDesc[$this->action_status];
        }

        public function buyer()
        {
            return User::find($this->buyer_id);
        }

        public static function add(PointConsume $obj)
        {
            if ($obj->consumeable_type == 'BuyerBespeakDrive' || $obj->consumeable_type == 'BuyerClue') {
                $salesCustomer = self::whereBuyerId($obj->user_id)
                                    ->whereSalesId($obj->sales_id)
                                    ->whereDealerId($obj->dealer_id)
                                    ->first();

                $lowest_quote = 0;

                if($obj->consumeable_type == 'BuyerClue' && $obj->abled()->category == 'lowest_quote')
                {
                    $lowest_quote = 1;
                }

                if (!$salesCustomer) {
                    $salesCustomer = self::saveData([
                        'buyer_id' => $obj->user_id,
                        'sales_id' => $obj->sales_id,
                        'dealer_id' => $obj->dealer_id,
                        'lowest_quote'  => $lowest_quote,
                        'unread' => 1,
                        'action_status_updated_at' => date('Y-m-d H:i:s'),
                    ]);
                    SalesCustomerRelation::add($salesCustomer, $obj);
                } else {
                    $salesCustomer->unread = 1;
                    $salesCustomer->action_status = 0;
                    $salesCustomer->lowest_quote = $lowest_quote;
                    $salesCustomer->action_status_updated_at = date('Y-m-d H:i:s');
                    $salesCustomer->save();
                }

                $sales = Sales::findByUserId($obj->sales_id);
                $sales->giveGiftCardToBuyer($obj->user_id);
            }
        }

        public static function createFromReverseBid(Precedent $obj)
        {
            $salesId = $obj->sales_id;
            $buyerId = $obj->buyer_id;
            $dealerId = Sales::findByUserId($salesId)->dealer_id;

            $salesCustomer = self::whereBuyerId($buyerId)
                                    ->whereSalesId($salesId)
                                    ->whereDealerId($dealerId)
                                    ->first();

            if (!$salesCustomer) {
                $data = [
                    'sales_id' => $salesId,
                    'dealer_id' => $dealerId,
                    'buyer_id' => $buyerId,
                    'unread' => 1,
                    'important' => 1,
                    'action_status_updated_at' => date('Y-m-d H:i:s'),
                ];
                self::saveData($data);
            } else {
                $salesCustomer->unread = 1;
                $salesCustomer->important = 1;
                $salesCustomer->action_status = 0;
                $salesCustomer->action_status_updated_at = date('Y-m-d H:i:s');
                $salesCustomer->save();
            }
        }

        public static function scopeSalesIdAndDealerId($query, $salesId)
        {
            $sales = Sales::findByUserId($salesId);
            return $query->whereSalesID($salesId)->whereDealerId($sales->dealer_id);
        }

        /**
         * 销售线索统计
         * @return [type] [int]
         */
        public static function point_consumes_total($salesId, $DealerId, $start_time = null, $end_time = null)
        {
            $start_time = !$start_time?date("Y-m-d",0):$start_time;
            $end_time = !$end_time?date("Y-m-d"):$end_time;

            $point_consumes_total = self::whereSalesId($salesId)
                        ->whereDealerId($DealerId)
                        ->where('action_status_updated_at','>=',$start_time)
                        ->where('action_status_updated_at','<=',$end_time)
                        ->count();

            return $point_consumes_total;
        }

        /**
         *  已读最低价状态
         * @return [type]          [description]
         */
        public function readLowestQuote(){
            $this->lowest_quote = self::READ_LOWEST_QUOTE;
            $this->save();
        }
        /**
         * 该销售是最低价
         * @return boolean [description]
         */
        public function isLowestQuote(){
            $this->lowest_quote = self::IS_LOWEST_QUOTE;
            $this->save();
        }

        /**
         * [SalesCustomerCntByBuyerClue description]
         * @author limao 2015-05-11
         * @param  CityBrand $cityBrand  [description]
         * @param  [type]    $start_time [description]
         * @param  [type]    $end_time   [description]
         */
        public static function SalesCustomerCntByBuyerClue(CityBrand $cityBrand, $start_time, $end_time)
        {
            $initiative_customer_cnt   = 0;
            $lowest_quote_customer_cnt = 0;

            $dealer_ids = Dealer::GetIdsByCityBrand($cityBrand);

            if($dealer_ids){
                $query = self::distinct()
                        ->whereIn('dealer_id',$dealer_ids)
                        ->where('created_at','>=',$start_time)
                        ->where('created_at','<',date("Y-m-d",strtotime($end_time . " +1 day")));

                $initiative_customer_cnt   = $query->clone()->where('lowest_quote',0)->count(['buyer_id','dealer_id']);
                $lowest_quote_customer_cnt = $query->clone()->where('lowest_quote',1)->count(['buyer_id','dealer_id']);
            }
            return compact('initiative_customer_cnt','lowest_quote_customer_cnt');
        }

    }
```

    /**
     * 该销售是最低价
     * @return boolean [description]
     */
    public function isLowestQuote(){
        $this->lowest_quote = self::IS_LOWEST_QUOTE;
        $this->save();
    }
